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

(some intro text)

## Background: Using CircleCI for Ansible Playbook Commit Checks

We use Ansible and Terraform to manage some of our internal infrastructure. All configurations are stored in Git. All changes to that configuration must be submitted as a Pull Request. All PRs must be approved by at least one reviewer, and all commit checks must pass. We use [CircleCI](https://circleci.com) to run these commit checks.

We run multiple checks, but for Ansible playbooks, they include using `ansible-lint`, and `ansible-playbook --syntax-check`. We then spin up a Docker container using CircleCI and run some of our playbooks **twice**, checking that it passes both times, _and_ that the second run records no changes.

Here's a snippet of some of our CircleCI configuration:

(put in snippet of previous config)

This system has been working well for us.  Once a PR has passed checks, been approved and merged, it will be picked up by all production systems.

## Enter Systemd

This system worked up until the point I went poking into our [Sensu](https://sensu.io) playbooks, and saw that we were using an old Sensu repo. We use these to install Sensu clients on our EC2 instances, and configure checks. 

We were stuck on the 0.26.x version. This was no big deal - for our simple needs, running that version was fine, and worked as expected. But I decided to get up to date, and fix the repo URL, and pull in current package versions (1.6.x at time of writing).

To test it, I first made a local Ansible change, and ran it on a test system. Perfect - new repo added, new Sensu version installed. A few minor tweaks to account for different plugin install methods, and everything worked.

Next step, submit a PR.

Bzzzt:


(add CircleCI/Systemd/Sensu error here)

Hmm.

## Systemd + Docker: Not Friends

What's going on here? Basically: Systemd + Docker do not get along. Somewhere along the way Sensu had changed to use Systemd to manage the service, rather than traditional init scripts. In theory this should not be a problem - we are using Ubuntu 16.04, and systemd is no issue there. 

The problem was with our test infrastructure. We were launching an Ubuntu 16.04 Docker container, and running our tests inside that.

Docker + Systemd do not get along:

(insert links to various blogs describing the problem)

## OK, Here's a Workaround:

(privileged containers + cgroup)




## Nope: Still Not working

Sadly that didn't work. The NTP role we are using started failing. 

(ntp errors)

## Finally: Machine Executor + Disable AppArmor

CircleCI supports different "executors" - the environment the tests run in. Normally you run test in a Docker container, and just specify which container image you want to use.

But it also supports the `machine` executor. Rather than run tests in a container, it runs test within a VM. Problem is, those VMs are Ubuntu 14.04-based. I needed to test with Ubuntu 16.04.

Here's the trick: Use the `machine` executor, and launch Ubuntu 16.04 Docker containes inside that.

Here's my new config:

(updated config)

...and now I get a different error. NTPD is not working:

(NTP error)

OK, it's a different error this time. Here's what I had to do to successfully run tests that launch Systemd-managed services in Ubuntu 16.04, running in a Docker container:

(config)

Success at last! Tests finally passed, and I was able to move on to the next round of Ansible improvements I wanted to make.

{% include note.html content="I was feeling a bit over it all by the time I finally got all this working. If you know of a way to modify AppArmor rather than disabling it, let me know. I wasn't too worried due to the low risk of what I'm doing here, along with the ephemeral nature of the Machine instances." %}
