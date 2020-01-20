---
author: lindsay
date: 2020-01-19
layout: post
slug: qfx-host-version
title: QFX Upgrades - Check Host Version
categories:
- Routing &amp; Switching
tags:
- Juniper
---

I came across a situation where a software upgrade failed for some members in a [Juniper QFX Virtual Chassis](https://www.juniper.net/documentation/en_US/junos/topics/concept/virtual-chassis-switches-overview.html). There is a known issue with upgrades with a certain configuration + version combination, but I thought it didn't apply to me. Turns out that the key was the **host** OS version, not the Junos VM version. Your host and guest versions can be out of sync with Juniper QFX 5K devices, and this can lead to confusing behavior, especially in a virtual chassis where host OS versions might vary.

## Upgrade Failures - post-install error

When upgrading an old Juniper QFX5100, you might see these messages when running the upgrade:

```text
Error: jinstall-vjunos fails post-install
Error: jinstall-vjunos-14.1X53-D34-domestic-signed fails post-install
```

In my case, I saw it for some nodes in a Virtual Chassis. Some worked, some failed. [KB31923](https://kb.juniper.net/InfoCenter/index?page=content&id=KB31923) says that this error is due to this configuration:

```shell
# show system internet-options
tcp-drop-synfin-set;
no-tcp-reset drop-tcp-with-syn-only;
```

Easy enough to change:

```shell
# delete system internet-options
{master:0}[edit]
root# show |compare
[edit system]
  internet-options {
      tcp-drop-synfin-set;
     no-tcp-reset drop-tcp-with-syn-only;
 
{master:0}[edit]
root# commit
```

Then the upgrade will proceed.

## But I'm not using that version?

The KB says that it applies to "...Junos 14.1X53-D25, 27, 30 and 40..." - but I wasn't running that version. I was running something slightly newer. Or was I?

The QFX5100 runs a hypervisor, and uses KVM to launch Junos as a guest VM. Normally you interact with the switch via the guest VM. When you check the version, you run `show version`, and look at the first `Junos: xxx` line. That's not enough, because you might end up in a situation where the guest version is newer than the host version, e.g.:

```
lkhill@qfx5100> show version
fpc0:
--------------------------------------------------------------------------
Hostname: qfx5100
Model: qfx5100-48s-6q
Junos: 14.1X53-D42.3
<snip>
JUNOS Host Software [13.2X51-D38]
Junos for Automation Enhancement
```

Huh. The guest version is all that matters for forming a virtual chassis, so they don't care about the host version mismatch. But when you go to upgrade...it fails on the systems running the older host OS.

## How did we end up in this situation?

The key here is that the guest version does not have to match the host version. From the [Juniper docs](https://www.juniper.net/documentation/en_US/junos/topics/task/installation/qfx-series-software-upgrading-cli.html):

> Note: On QFX5100 and EX4600 switches, the Host OS is not upgraded automatically, so you must use the force-host option if you want the Junos OS and Host OS versions to be the same.
> ...
> * The Junos OS and Host OS versions do not need to be the same.
> ...
> * Upgrading the Host OS is not required for every software upgrade, as noted above.

If you do a format install from USB, it will make both versions the same. But if you do an upgrade using `request system software add ...`, then...well...it gets messy.

During the upgrade process, it checks the current host version. It has rules to check that host version against the target guest version. I have not found where this is documented, but there are rules around minimum host versions for guests. If you upgrade from 13.1 to 14.1, it won't change the host version. If you upgrade from 14.1 to 17.3, it will. If you upgrade from 17.3 to 18.4, it won't. 

## This could cause trouble?

Yes, it could. Mostly it is fine, and for a standalone switch, it doesn't really matter. But in a virtual chassis, it can lead to unexepected behavior, such as upgrades failing on only some nodes. I also saw that `show version` would hang for a few seconds on the nodes running older host OS versions.

## What should I do?

For standalone devices, it doesn't matter much. But for virtual chassis members in particular, I recommend adding `force-host` to the `request system software add` command. This will force a host upgrade, regardless of if Junos thinks it needs to or not. It will mean a slightly longer reboot time, but it ensure that all devices are running a consistent set of guest and host versions. Will help avoid some confusion further down the line.