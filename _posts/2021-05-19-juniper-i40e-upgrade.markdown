---
author: lindsay
date: '2021-05-19 21:30 -07:00'
layout: post
slug: juniper-i40e-upgrade
title: Juniper i40e NVM Firmware Upgrade
categories:
- Routing &amp; Switching
tags:
- Juniper
---

Juniper Routing Engines with VM Host need an [i40e NVM firmware upgrade](https://kb.juniper.net/InfoCenter/index?page=content&id=TSB17603). The procedure is a pain in the ass, and documentation is not great. But you can't avoid the upgrade any more. New Junos versions need the firmware upgrade, and replacement REs will [ship with it already installed](https://kb.juniper.net/InfoCenter/index?page=content&id=TSB17978). Here's some tips on doing the upgrade.

## Background

Newer Juniper Routing Engines use a [Linux-based hypervisor](https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade/topics/topic-map/vm-host-overview.html), and Junos (still BSD-based) runs as a guest VM. This is mostly transparent for day to day operations. When you do a Junos upgrade, it will upgrade the underlying hypervisor if required.

Upcoming Junos versions ship with a new version of Wind River Linux that needs i40e firmware version 6.01. Older versions used v4.26. You need the new i40e firmware installed first, before you can install the latest Junos versions. You can't put this upgrade off forever. Sooner or later you'll want to ugprade to a Junos version that only supports the new firmware. Or you'll get a replacement RE delivered with new firmware, and you can't downgrade it.

For the last couple of years, Juniper has been shipping Junos versions that will work with both old & new firmware versions. You need one of these to do the upgrade.

So you need to do something like this:

1. Upgrade to a recent-ish Junos version (e.g. 18.4R3) that supports old & new firmware.
1. Install the new firmware
1. Now you can upgrade to future versions that only support the new firmware.

## Firmware Upgrade Overview

As mentioned, the upgrade process is a hassle, especially for dual-RE systems, since you need to do at least 3 reboots per RE. Juniper tells you that you need console access and remote power control. It's nice to have console access, but you can get away without it. You definitely don't need remote power control for a dual-RE system, since you can power cycle the CB from the other RE.

Here's a bit more detailed look at the steps:

1. Log a TAC case to get the correct jfirmware package for your Junos version. Copy that and the LLDP package (available at [TSB17603](https://kb.juniper.net/InfoCenter/index?page=content&id=TSB17603)) to your router, and copy them to the backup RE.
1. Disable GRES.
1. Install the jfirmware package, and start the firmware upgrade.
1. Re-enable GRES and wait for sync to complete.
1. Fail over to RE1, and reboot RE0. You'll need to go through 3 reboot cycles. Use power controls to speed it up, or wait a bit longer.
1. Disable GRES to allow you to install the jfirmware package on RE1. After install, re-enable GRES
1. Fail over to make RE0 primary. Reboot RE1, go through 3 reboot cycles.
1. Install the LLDP package on each RE, and reboot them again.
1. Done - now you're set up for future Junos upgrades

Tedious, right? There are some ways to make it slightly less painful, and incorporate a Junos upgrade along the way, with just one traffic interruption due to FPC reloads. But it's a stupid process, and I hope I never have to go through this cycle again.

## jfirmware install and reboot

Install the jfirmware package: 

```shell
root@netboot> request vmhost software add /var/tmp/jfirmware-vmhost-x86-64-...
Verified jfirmware-vmhost-x86-64-18.4R3-S4.2 signed by PackageProductionEc_2018 method ECDSA256+SHA256
[ platform = ; re_name = RE-S-2X00x6 ]
Pushing /packages/db/jfirmware-vmhost-x86-64-18.4R3-S4.2/contents/vmhost-firmware-x86-64-18.4R3-S4.2.tgz to host ...done.
Extracting /packages/db/jfirmware-vmhost-x86-64-18.4R3-S4.2/contents/vmhost-firmware-x86-64-18.4R3-S4.2.tgz ...done.
Preparing... ##################################################
supported for kernel version: 3.10.100-ovp-rt110-WR6.0.0.38_preempt-rt
i40e_pkg ##################################################
Installation of /tmp/i40e_pkg-2.0-0.x86_64.rpm ... done
Installing i40e pkg on host ... done.
Preparing... ##################################################
supported for kernel version: 3.10.100-ovp-rt110-WR6.0.0.38_preempt-rt
bios_pkg ##################################################
Installing /tmp/bios_pkg-1.0-0.x86_64.rpm ... done.
Installing bios pkg on host ... done.
 
 
root@netboot>
```

This just makes it available for install - you still need to do kick off the install. Note that `request system firmware upgrade` is a hidden command, and you need to type it out. You can tab-complete the last part:

```shell
root@netboot> request system firmware upgrade re i40nvm
Part Type Tag Current Available Status
version version
Routing Engine 0 RE i40e-NVM 7 4.26 6.01 OK
Perform indicated firmware upgrade ? [yes,no] (no) yes
 
Firmware upgrade initiated, use "show system firmware" after vmhost reboot to verify the firmware version
 
root@netboot>
```

Now flip the master routing engine over, and reboot RE0 from RE1 with `request vmhost reboot routing-engine other`.

If you watch the console on RE0, you'll see this:

```shell
Initializing platform services:              2.4.3
NVM_version: 4.26
DRV_version: 2.4.3
Need Manual procedure to upgrade i40e firmware revision from 4.26 to  6.01
upgrading i40e firmware .....
 
Intel(R) Ethernet NVM Update Tool
NVMUpdate version 1.30.2.11
Copyright (C) 2013 - 2017 Intel Corporation.
 
Unsupported device found - DeviceId: 153A
Config file read.
Inventory
[00:005:00:00]: Intel(R) Ethernet Controller XL710 for 40GbE backplane
    Flash inventory started
    Shadow RAM inventory started
Warning: VPD is not valid
Alternate MAC address is not set
    Shadow RAM inventory finished
    Flash inventory finished
    OROM inventory started
    OROM inventory finished
[00:005:00:01]: Intel(R) Ethernet Controller XL710 for 40GbE backplane
    Device already inventoried.
Update
[00:005:00:00]: Intel(R) Ethernet Controller XL710 for 40GbE backplane
    Flash update started
|======================[100%]======================|
    NVM image verification started
    Shadow RAM image verification started
|======================[100%]======================|
    Shadow RAM image verification finished
    Flash image verification started
|======================[100%]======================|
    Flash image verification finished
    NVM image verification finished
    Flash update successful
Config file doesn't have any OROM components specified for device 'XL710'. Tool will use current device's combo set for the OROM update.
Config file doesn't have any OROM components specified for device 'XL710'. Tool will use current device's combo set for the OROM update.
Post update inventory
[00:005:00:00]: Intel(R) Ethernet Controller XL710 for 40GbE backplane
    EEPROM inventory started
Alternate MAC address is not set
    EEPROM inventory finished
    OROM inventory started
    OROM inventory finished
[00:005:00:01]: Intel(R) Ethernet Controller XL710 for 40GbE backplane
    Device already inventoried.
Please Power Cycle your system now and run the NVM update utility again to complete the update. Failure to do so will result in an incomplete NVM update.
Upgrade complete please power reboot
You may notify to  power reboot again after reboot if required
```

At this point you can power-cycle the CB, which will cut power cycle the RE. The first time you do this, you might see that it takes a while before `show chassis environment cb 0` shows it as Offline. Once it is Offline, bring it back online again. 

```shell
root@netboot> request chassis cb offline slot 0
Offline initiated, use "show chassis environment cb" to verify
 
root@netboot> show chassis environment cb 0
CB 0 status:
  State                      Offline
  Power 1                    Disabled
  Power 2                    Disabled
 
root@netboot> request chassis cb online slot 0
Online initiated, use "show chassis environment cb" to verify
 
root@netboot>
```

If you're watching the console, you'll see very similar messages to last time.

{% include tip.html content="What if you don't have a console server? How do you know when to power cycle the RE? If you leave it long enough, the routing engine will boot on its own. When you see it back up, power cycle it again." %}

Power cycle the CB again. Eventually you'll see this:

```shell
i40e firmware revision is the latest to 6.01

Fortville NVM Firmware Version: 6.01

Host reboot is required to load compatible i40e driver
```

You don't need to do anything at this point. Eventually it will carry on to load Junos.

You can check that the new firmware shows up:

```shell
root@netboot> show system firmware
Part Type Tag Current Available Status
version version
Routing Engine 0 RE BIOS 0 0.53.1 0.55.2 OK
Routing Engine 0 RE FPGA 1 41.0.0 41.0 OK
Routing Engine 0 RE SSD1 5 12028 12050 OK
Routing Engine 0 RE SSD2 5 12028 12050 OK
Routing Engine 0 RE i40e-NVM 7 6.1 6.01 OK
Routing Engine 1 0 0.0.0 0 OK

root@netboot>
```

Yes. Yes they did mix up "6.1" and "6.01". Don't worry about it. You're running the right version.

Of course you're not done yet, now you need to load the LLDP fix, and go through the whole process again with the other RE. 

## LLDP package install

On each RE, run this:

```shell
root@netboot> request system software add /var/tmp/lldp-patch-for-i40e-upgrade.tgz
Verified lldp-patch-for-i40e-upgrade signed by PackageDevelopmentEc_2018 method ECDSA256+SHA256
[ re_name = RE-S-2X00x6 ]
Pushing script(s) to host ...
Install the script(s) under host-os....
Script(s) copy done.

root@netboot> show version | match lldp
lldp-patch-for-i40e-upgrade

root@netboot>
```

Note that it's `request system software add ...`, not `request vmhost software add ...`

Then one more reboot of each RE, but just a regular reboot, with no need for any power cycling. When the system is back up, you'll see a message like this when loggging in:

```shell
FreeBSD/amd64 (netboot) (ttyu0)

login: root
Password:
Last login: Sun Oct 4 20:49:38 on ttyu0

--- JUNOS 18.4R3-S4.2 Kernel 64-bit JNPR-11.0-20200618.2bc7e35_buil
At least one package installed on this device has limited support.
Run 'file show /etc/notices/unsupported.txt' for details.
root@netboot:~ #
```

Don't worry about it. Juniper TAC assures me that this is still a supported configuration.

## Combining with Junos upgrade

Can I combine this with a Junos upgrade, and how do I minimise the number of FPC restarts, so I minimise user disruption?

Yes, if you pay attention to what you're doing, and you give yourself a little more time. You can do the firmware upgrades, LLDP fix and Junos upgrade in one change window, with only one disruptive FPC restart.

Here's how I would do it:

1. Disable GRES, and install the jfirmware package on RE0, but don't reboot yet.
1. Re-enable GRES and wait for the REs to sync
1. Fail over to RE1 - this should be seamless. 
1. Go through the 3 reboot cycles on RE0 to get the i40e firmware done
1. Install the LLDP package on RE0
1. Install the new Junos version on RE0 and reboot
1. Install the new jfirmware package on RE1.
1. Fail over to RE0. The new Junos version will take over, and all FPCs will restart. This is disruptive
1. Go through the reboot cycles on RE1. When i40e is done, install the LLDP package, followed by the new Junos version
1. Re-enable GRES

What about the LLDP fix with the new Junos version? No need to re-install it. Depending on which version you upgrade to, it will either still be there as a separate package, or it will be integrated into the main codebase.

The only good thing about this process? It worked on every RE I upgraded.

Hope this helps, and hope I never go through this again.
