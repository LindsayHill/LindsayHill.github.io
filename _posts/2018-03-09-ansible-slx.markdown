---
author: lindsay
date: 2018-03-09 16:56:41+00:00
layout: post
slug: ansible-slx
title: Ansible for Extreme Devices
categories:
- Automation
tags:
- networking
- ansible
---

Here's something I've been working on recently: Ansible modules for Extreme SLX switches & routers. Ansible is a popular automation framework, and with good reason: it has a low barrier to entry. Time to usefulness is short. But you need device-specific modules to work with networking devices. Finally we have some modules for SLX. Read on for how to use them.

> This blog is not an intro to Ansible in general. There's plenty of good intros out there. This is specifically about demonstrating Ansible with SLX switches.

## Background

Ansible is an agent-less configuration management system. It uses "playbooks", written in YAML, to define desired configuration state. "modules" written in Python translate this into whatever is needed to configure the system, application, database or network device. 

Ansible has been making great strides in adding network automation capabilities. But we haven't had any modules for working with <s>Brocade</s> Extreme devices. That is now changing. 

[PaulQuack](https://github.com/PaulQuack) has contributed MLXe (Ironware) [modules](http://docs.ansible.com/ansible/devel/modules/list_of_network_modules.html#ironware), which will go GA in Ansible 2.5 (due for release in March 2018). And I've been working on modules for the SLX, with my colleagues. These have not yet been merged upstream, but it's Open Source, so you can grab them and try them out.

Here's the modules available so far:

* Command - `slxos_command`
* Config - `slxos_config`
* Facts - `slxos_facts`
* VLAN - `slxos_vlan`
* Interface - `slxos_interface`
* LinkAgg - `slxos_linkagg`
* LLDP - `slxos_lldp`

Here's how to install & use these modules:

## Installation

Here's how to install Ansible in your home directory, using our latest development branch:

> NB in future you will be able to install Ansible using [normal Ansible installation procedures](https://docs.ansible.com/ansible/intro_installation.html). Our modules have not yet been merged upstream.

```shell
git clone -b slxos_modules https://github.com/StackStorm/ansible
cd ansible
virtualenv venv
.  ./venv/bin/activate
pip install -r requirements.txt
```

## Configuration

Add entries to `/etc/hosts` file for your switches - e.g. `172.16.10.42 slx01`

Create `~/.ansible.cfg` containing this:
```ini
[defaults]
ansible_python_interpreter=~/ansible/venv/bin/python
host_key_checking = False
inventory = ~/playbooks/hosts 
```

Create these directories: `~/playbooks`, `~/playbooks/backups`, `~/playbooks/group_vars`.

Create the file `~/playbooks/hosts` containing this:
```ini
[slx]
slx01 ansible_network_os=slxos ansible_connection=network_cli
```

If you have more than one switch (e.g. `slx02`, `slx03`, add additional lines as required)

Create the file `~/playbooks/group_vars/slx.yaml` containing this:
```yaml
---
ansible_user: admin
ansible_ssh_pass: password
```

Replace `password` with the password you use to authenticate to your SLX devices. Yes, you should be using `ansible-vault` for this file.

The above commands only need to be run the first time you do setup. Next time you login, you just need to run these commands:

```shell
cd ansible
. ./venv/bin/activate
. ./hacking/env-setup
```

## Command Module:

Now we've got things installed & configured, here's how to use it.

First up is the `slxos_command` module. It is used to run commands on SLX devices - e.g `show` commands. It is not for use with configuration commands. The syntax is very similar to [ios_command](http://docs.ansible.com/ansible/latest/ios_command_module.html).

This can be used to do things like run `show chassis`, and capture the output. This output can be used in further tasks, displayed to screen, or written to file. It can also look for specific lines in the output, and fail if those lines are not present. This is useful if you want to do things like check NTP synchronisation state.

All commands below are run from the `~/playbooks` directory.

### Run `show version` and publish output:
    
Create `slx_command_ver.yaml`, containing this:
```yaml
---
- hosts: slx01
  gather_facts: no

  tasks:
    - name: Grab version
      slxos_command:
        commands: "show version"
      register: show_ver

    - name: var_result
      debug:
        var: show_ver.stdout_lines[0]
```
    
Run it with `ansible-playbook slx_command_ver.yaml`. Here's some example output:
    
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook slx_command_ver.yaml
   
PLAY [slx01] *************************************************************************************************************************
   
TASK [Grab version] ******************************************************************************************************************
ok: [slx01]
   
TASK [var_result] ********************************************************************************************************************
ok: [slx01] => {
    "failed": false,
    "show_ver.stdout_lines[0]": [
        "SLX-OS Operating System Software",
        "SLX-OS Operating System Version: 17s.1.02",
        "Copyright (c) 1995-2018 Brocade Communications Systems, Inc.",
        "Firmware name:      17s.1.02",
        "Build Time:         00:06:59 Sep 28, 2017",
        "Install Time:       10:58:51 Sep 30, 2017",
        "Kernel:             2.6.34.6",
        "Host Version:       Ubuntu 14.04 LTS",
        "Host Kernel:        Linux 3.14.17",
        "",
        "Control Processor:   QEMU Virtual CPU version 2.0.0",
        "",
        "System Uptime:   143days 19hrs 29mins 7secs ",
        "",
        "Slot    Name    Primary/Secondary Versions                         Status",
          "---------------------------------------------------------------------------",
        "SW/0    SLX-OS  17s.1.02                                           ACTIVE*",
        "                17s.1.02"
    ]
}
   
PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=3    changed=0    unreachable=0    failed=0
```
    
### Run a command and check the output

In this example, we're checking for specific NTP servers in the output. On my device there is only one NTP server configured and in use. This playbook should pass the first task, and fail the second one.
    
Here's what's configured on my device:
```shell
SLX01# show run | inc ntp
ntp server 172.16.10.2 use-vrf mgmt-vrf
SLX01#
```
Create `slx_command_ntp.yaml`:
```yaml
---
- hosts: slx01
  gather_facts: no
  
  tasks:
    - name: Check ntp status
      slxos_command:
        commands: "show ntp status"
        wait_for: result[0] contains 172.16.10.2
        retries: 1

    - name: Check for non-existent NTP server
      slxos_command:
        commands: "show ntp status"
        wait_for: result[0] contains 172.16.10.3
        retries: 1
```
Example output:
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook slx_command_ntp.yaml
   
PLAY [slx01] *************************************************************************************************************************
   
TASK [Check ntp status] **************************************************************************************************************
ok: [slx01]
   
TASK [Check for non-existent NTP server] *********************************************************************************************
fatal: [slx01]: FAILED! => {"changed": false, "failed": true, "failed_conditions": ["result[0] contains 172.16.10.3"], "msg": "One or more conditional statements have not been satisfied"}
to retry, use: --limit @/home/lhill/playbooks/slx_command_ntp.retry
   
PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=1    changed=0    unreachable=0    failed=1
   
(venv) lhill@bwc:~/playbooks$
```
Note that this is the expected behavior - that second task **should** fail.

## Config Module:

This module is used to manage configuration sections on SLXes. It is not for use with `show` commands. The syntax is very similar to [ios_config](http://docs.ansible.com/ansible/latest/ios_config_module.html). It is used to ensure config lines are present. These can be at the global level, or under specific parents (e.g. under an `interface Ethernet` stanza or an access-list). You can also run commands before or after changing lines. This is useful when updating ACLs.

This module will report if any changes were made. This is very handy for auditing devices - you can keep re-running the same playbook, and identify which devices had unauthorized changes made. It can also be run in `check` mode, where it reports differences, but does not make changes. 

You can choose if you want to save the configuration after making changes. There are four options for `save_when`:

* `never` - this is the default. Don't save changes.
* `always` - always save the config, even if nothing changed
* `changed` - save the config only if this playbook run changed it. Note the difference between this behavior and `modified` below.
* `modified` - save the config if the running and startup configs are different. This is handy if someone else has been making changes without saving them.

### Set a global configuration value

This example sets an NTP server if not already set, and saves the configuration if we change it.

Create `slx_config_set_ntp.yaml`:
```yaml
---
- hosts: slx01
  gather_facts: no

  tasks:
    - name: NTP server 172.16.10.3
      slxos_config:
        lines: "ntp server 172.16.10.3 use-vrf mgmt-vrf"
        save_when: changed
```

This was our config before running the playbook:
```
SLX01# show run | inc ntp
ntp server 172.16.10.2 use-vrf mgmt-vrf
SLX01#
```
Example output (note the added `-vv` here to give us additional information about what's happening):
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook -vv slx_config_set_ntp.yaml
ansible-playbook 2.6.0 (slxos_modules 2a7acba239) last updated 2018/02/21 02:06:16 (GMT +200)
  config file = /home/lhill/.ansible.cfg
  configured module search path = [u'/home/lhill/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /home/lhill/ansible/lib/ansible
  executable location = /home/lhill/ansible/bin/ansible-playbook
  python version = 2.7.12 (default, Dec  4 2017, 14:50:18) [GCC 5.4.0 20160609]
Using /home/lhill/.ansible.cfg as config file

PLAYBOOK: slx_config_set_ntp.yaml ****************************************************************************************************
1 plays in slx_config_set_ntp.yaml

PLAY [slx01] *************************************************************************************************************************
META: ran handlers

TASK [NTP server 172.16.10.3] ********************************************************************************************************
task path: /home/lhill/playbooks/slx_config_set_ntp.yaml:8
changed: [slx01] => {"changed": true, "commands": ["ntp server 172.16.10.3 use-vrf mgmt-vrf"], "failed": false, "updates": ["ntp server 172.16.10.3 use-vrf mgmt-vrf"]}
META: ran handlers
META: ran handlers

PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=1    changed=1    unreachable=0    failed=0

(venv) lhill@bwc:~/playbooks$
```
Now look at the startup and running configs on the device:
```
SLX01# show run | inc ntp
ntp server 172.16.10.2 use-vrf mgmt-vrf
ntp server 172.16.10.3 use-vrf mgmt-vrf
SLX01# show startup-config | inc ntp
ntp server 172.16.10.2 use-vrf mgmt-vrf
ntp server 172.16.10.3 use-vrf mgmt-vrf
SLX01#
```
We can see our command has been added **and saved**.

Now run the playbook again:
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook -v slx_config_set_ntp.yaml
Using /home/lhill/.ansible.cfg as config file

PLAY [slx01] *************************************************************************************************************************

TASK [NTP server 172.16.10.3] ********************************************************************************************************
ok: [slx01] => {"changed": false, "failed": false}

PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=1    changed=0    unreachable=0    failed=0

(venv) lhill@bwc:~/playbooks$
```
Note the output: `changed=0`. It sees that the line is already there, and does not need to be changed.

### Removing a configuration line

The `slxos_config` module is not declarative. By default it will not remove additional lines. There are ways around this with more complex playbooks. That's an exercise for the reader. But you can use `no` statements. Be aware that this will **always** report changes, because it sees that command as 'not present' so runs it every time.

Create `slxos_config_remove_ntp.yaml`:
```yaml
---
- hosts: slx01
  gather_facts: no
  
  tasks:
    - name: NTP server 172.16.10.3
      slxos_config:
        lines: "no ntp server 172.16.10.3 use-vrf mgmt-vrf"
```

Output:
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook -v slx_config_remove_ntp.yaml
Using /home/lhill/.ansible.cfg as config file

PLAY [slx01] *************************************************************************************************************************

TASK [NTP server 172.16.10.3] ********************************************************************************************************
changed: [slx01] => {"changed": true, "commands": ["no ntp server 172.16.10.3 use-vrf mgmt-vrf"], "failed": false, "updates": ["no ntp server 172.16.10.3 use-vrf mgmt-vrf"]}

PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=1    changed=1    unreachable=0    failed=0

(venv) lhill@bwc:~/playbooks$
```

### More complex configuration using `parents`

This example has multiple tasks - first it grabs the running config, and saves it to a local timestamped backup file. Then it makes a configuration change, but only on one specific interface. This uses the `parents` option. Also note the use of `gather_facts: yes` to get the current date & time, which is then used in the variable `ansible_facts.date_time.iso8601`. Here we're going to enable PTP on a specific interface:

`slx_config_parents.yaml`:
```yaml
---
- hosts: slx01
  gather_facts: yes
  
  tasks:
    - name: Get running config
      slxos_command:
        commands: show run
      register: show_run

    - name: Save config to file
      copy:
        content: "{{ show_run.stdout[0] }}"
        dest: "backups/{{inventory_hostname}}-{{ansible_facts.date_time.iso8601}}.txt"

    - name: PTP on Eth 0/24
      slxos_config:
        lines: protocol ptp
        parents: interface Ethernet 0/24
        save_when: modified
```
Example output:
```shell
(venv) lhill@bwc:~/playbooks$ ansible-playbook slx_config_parents.yaml

PLAY [slx01] *************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************
ok: [slx01]

TASK [Get running config] ************************************************************************************************************
ok: [slx01]

TASK [Save config to file] ***********************************************************************************************************
changed: [slx01]

TASK [PTP on Eth 0/24] ***************************************************************************************************************
changed: [slx01]

PLAY RECAP ***************************************************************************************************************************
slx01                      : ok=4    changed=2    unreachable=0    failed=0

(venv) lhill@bwc:~/playbooks$
```
We can see that we have a backup file saved locally:
```shell
(venv) lhill@bwc:~/playbooks$ head backups/slx01-2018-02-21T06\:55\:58Z.txt
root enable
host-table aging-mode conversational
clock timezone Etc/GMT
hardware
 profile tcam default
 profile overlay-visibility default
 profile route-table default maximum_paths 8
 system-mode default
!
http server use-vrf default-vrf
(venv) lhill@bwc:~/playbooks$
```
And we can see that our `protocol ptp` command was added to one specific interface:
```
SLX01# show run | begin ^interface\ Ethernet\ 0/23
interface Ethernet 0/23
 shutdown
!
interface Ethernet 0/24
 protocol ptp
 !
 shutdown
!
interface Ethernet 0/25
 shutdown
!
```

## More Examples

For more examples, check out this [repo](https://github.com/ExtremeNetworks/ansible-extreme). That has example playbooks, instructions on how to run them, and sample output. Examples include use of the Interface, VLAN, Facts, LLDP and LinkAgg modules.

## Future Development

Current plans are:

* Get initial modules accepted into Ansible upstream.
* Add more modules - probably banner, VRF, system, users, logging next.
* Monitor upstream Ansible developments and move to a REST API plugin if that becomes an option for network devices.
* Create more playbooks and roles for managing SLX switches.
* Develop modules for other Extreme systems - e.g. EXOS-based systems.

This is Open Source development folks - that means you can contribute to! Want a new module? Start writing one. See a bug? File an issue, or better yet, a Pull Request. Created some interesting playbooks? Let us know so they can get included in the examples repo.
