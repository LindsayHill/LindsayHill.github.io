---
author: lindsay
date: 2019-01-01 14:31:54+00:00
layout: post
slug: circleci-docker-systemd
title: CircleCI, Docker and Systemd
categories:
- Coding
tags:
- CircleCI
- Ansible
---

I have been battling to get the combination of CircleCI, Docker and systemd to play together. After much frustration, I have a workable solution. Machine Executor, `privileged: true`, cgroup passthrough, and disabling AppArmor.

# Background: CircleCI for Ansible Linting & Checks

In the StackStorm team we use CircleCI with most of our repositories. We check things like code style checks, and run unit tests. With every Pull Request we trigger these checks, and checks must pass before merging. Some repos also use CircleCI for post-merge deployment steps.

We use Ansible and Terraform to manage some of our internal infrastructure. All configurations are stored in Git. All changes to that configuration must be submitted as a Pull Request. All PRs need approval, and all commit checks must pass. We use [CircleCI](https://circleci.com) to run these commit checks.

We run multiple checks, but for Ansible playbooks, they include using `ansible-lint`, and `ansible-playbook --syntax-check`. We then spin up a Docker container using CircleCI and run some of our playbooks **twice**, checking that it passes both times, _and_ that the second run records no changes.

Here's a snippet of some of our CircleCI configuration:

```yaml
version: 2
jobs:
  build:
    working_directory: ~/ci
    docker:
      - image: williamyeh/ansible:ubuntu16.04
    steps:
      - checkout
      - run:
          name: Install ansible-lint
          command: |
            pip install ansible-lint
      - run:
          name: Ansible YAML syntax check
          command: |
            ansible-playbook --syntax-check *.yml
      - run:
          name: Ansible-lint check
          command: |
            ansible-lint -v *.yml
      - run:
          name: Run local.yml playbook in CircleCI Ubuntu16 env
          command: |
            ansible-playbook -vv local.yml
      - run:
          name: Run local.yml again, checking to make sure it's idempotent
          command: >
            ansible-playbook local.yml
            | grep 'changed=0.*failed=0'
            && (echo 'Idempotence test: pass' && exit 0)
            || (echo 'Idempotence test: fail' && exit 1)
```

This system has been working well for us.  Once a PR passes checks, gets approved and merged, all production systems pull it automatically.

## Enter Systemd

This system worked up to the point when I went poking into our [Sensu](https://sensu.io) playbooks, and saw that we were using an old Sensu repo. We use Sensu to run various monitoring checks on our EC2 instances and services.

We were stuck on the 0.26.x version. This was no big deal - for our simple needs, running that version was fine, and worked as expected. But I decided to get up to date, and fix the repo URL, and pull in current package versions (1.6.x at time of writing).

To test it, I first made a local Ansible change, and ran it on a test system. Perfect - new repo added, new Sensu version installed. A few minor tweaks to account for updated plugin installation, and everything worked.

Next step, submit a PR.

Bzzzt:

```shell
TASK [stackstorm.sensu : Ensure sensu-client is enabled and running] ***********
task path: /root/ci/roles/stackstorm.sensu/tasks/main.yml:54
fatal: [localhost]: FAILED! => {"changed": false, "msg": "Could not find the requested service sensu-client: host"}
```

Hmm. Why is it failing to check that service status? It works on a real system.

## Systemd + Docker: Not Friends

What's going on here? Basically: Systemd + Docker do not get along. At some point Sensu switched to using Systemd unit files to manage the service, rather than traditional init scripts. In theory this should not be a problem with Ubuntu 16.04. It should be better, making things consistent with the 'new' way of managing services.

The problem was with our test infrastructure. We were launching an Ubuntu 16.04 Docker container, and running our tests inside that.

Docker + Systemd do not get along. This [LWN](https://lwn.net/Articles/676831/) has a good summary of what's going on. A couple of choice quotes:

> "This is Lennart Poettering," said Walsh, showing a picture. "This is Solomon Hykes", showing another. "Neither one of them is willing to compromise much. And I get to be in the middle between them."
>
> ...
>
> According to Walsh's presentation, the root cause of the conflict is that the Docker daemon is designed to take over a lot of the functions that systemd also performs for Linux. These include initialization, service activation, security, and logging. "In a lot of ways Docker wants to be systemd," he claimed. "It dreams of being systemd."

## OK, Here's a Workaround

There is a workaround: You can start the container in "privileged" mode. This is insecure, but in theory should be fine for my CI environment.

I can't change the Docker setup options when I'm using `docker:` in my CircleCI configuration, but I can use `setup_remote_docker`, and launch Docker containers from my CI container.

So I tried doing something like this in my CircleCI config:

```yaml
    - setup_remote_docker:
        version: 18.05.0-ce
    - run:
        name: Pull dependent Docker Images
        command: docker-compose pull xenial
    - run:
        name: Set up remote container
        command: |
        docker-compose up -d xenial
    - run:
        name: Run local.yml playbook in remote Docker container
        command: |
        docker-compose exec xenial ansible-playbook -vv local.yml
```

I used this `docker-compose.yml` file:

```yaml
xenial:
  image: stackstorm/packagingtest:xenial-systemd
  privileged: true
  volumes:
    - /sys/fs/cgroup:/sys/fs/cgroup:ro
```

## Nope: Still Not working

Sadly that didn't work. The NTP role we are using started failing.

```shell
TASK [geerlingguy.ntp : Ensure NTP is running and enabled as configured.] ******
task path: /root/.ansible/geerlingguy.ntp/tasks/main.yml:23
fatal: [localhost]: FAILED! => {"changed": false, "msg": "Unable to start service ntp: Job for ntp.service failed because the control process exited with error code. See \"systemctl status ntp.service\" and \"journalctl -xe\" for details.\n"}
```

It's not really a privileged container. There are still some host-imposed restrictions.

## Finally: Machine Executor + Disable AppArmor

CircleCI supports different "executors" - the environment the tests run in. Normally you run tests in a Docker container, and just specify which container image you want to use.

But it also supports the [machine](https://circleci.com/docs/2.0/configuration-reference/#machine) executor. Rather than run tests in a container, it runs test within a VM. Problem is, those VMs are Ubuntu 14.04-based. I needed to test with Ubuntu 16.04.

Here's the combination I needed to run my tests: Use the `machine` executor, and launch Ubuntu 16.04 Docker containers from that VM. Because I have full control within the VM, I can set whatever options I need for my container.

Here's my new config:

```yaml
version: 2
jobs:
  ansible:
    machine: true
    working_directory: ~/ci
    steps:
      - checkout
      # All Ansible-related tests must run in a remote, privileged container
      - run:
          name: Set up host system
          command: |
            sudo service apparmor teardown
            sudo -H pip install docker-compose
      - run:
          name: Pull dependent Docker Images
          command: docker-compose pull xenial
      - run:
          name: Set up remote container
          command: |
            docker-compose up -d xenial
            docker-compose exec xenial sed -i 's/localhost/localhost ansible_connection=local/' /etc/ansible/hosts
      - run:
          name: Install ansible-lint
          command: |
            docker-compose exec xenial pip install ansible-lint
      - run:
          name: Ansible YAML syntax check
          command: |
            docker-compose exec xenial ansible-playbook --syntax-check *.yml
      - run:
          name: Ansible-lint check
          command: |
            docker-compose exec xenial ansible-lint -x 204 -v *.yml
      - run:
          name: Run local.yml playbook in remote Docker container
          command: |
            docker-compose exec xenial ansible-playbook -vv local.yml
      - run:
          name: Run local.yml again, checking to make sure it's idempotent
          command: >
            docker-compose exec -T xenial ansible-playbook local.yml
            | grep 'changed=0.*failed=0'
            && (echo 'Idempotence test: pass' && exit 0)
            || (echo 'Idempotence test: fail' && exit 1)
```

Success at last! Tests finally passed, and I was able to move on to the next round of Ansible improvements I wanted to make.

Note this part: `sudo service apparmor teardown`. Without this, NTPD inside my container failed to start. Same issue as [here](https://discuss.circleci.com/t/apparmor-error-running-privileged-docker-container-on-machine-executor/24564)

{% include note.html content="I was feeling a bit over it all by the time I finally got all this working. I wasn't too worried due to the low risk of what I'm doing here, along with the ephemeral nature of the Machine instances. But if you know the right way to change AppArmor rather than disabling it, let me know." %}