---
author: lindsay
date: 2020-03-21
layout: post
slug: mx-upgrade-overheat
title: Juniper MX Upgrades Causing Overheating
categories:
- Routing &amp; Switching
tags:
- Juniper
---

Juniper changed the way they do temperature management on MX240 and MX480 chassis devices, somewhere between 15.1 and 17.3. The net result is that your chassis might run hotter after you upgrade, which can lead to the system shutting down some optics. Probably not what you want. Luckily there's a few hidden commands you can use to change this behavior

## "Optics will be disabled..."

Post upgrade, you might see higher temperatures reported by `show chassis fpc`. This system was reporting temperatures in the low 30s, now it reports 50:

```shell
lindsayh@MX240> show chassis fpc
                     Temp  CPU Utilization (%)   CPU Utilization (%)  Memory    Utilization (%)
Slot State            (C)  Total  Interrupt      1min   5min   15min  DRAM (MB) Heap     Buffer
  0  Empty
  1  Online            50     22          1       22     22     22    2048       38         21
  2  Empty

{master}
lindsayh@MX240>
```

On its own, that's OK, until you start seeing log messages like this:

```text
FPC 1 temperature over 50 degrees C; non-high-temperature tolerant optics will be disabled in 58 seconds if condition persists
```

Yeah that's not good, especially when it carries out the threat, and starts disabling optics.

## Fix 1: Raise the threshold at which optics get disabled

[NEBS](https://en.wikipedia.org/wiki/Network_Equipment-Building_System)-compliant optics have a higher temperature threshold, so they're OK with the higher system temperature. But some of your optics might not officially be NEBS-compliant, and Juniper will shut them down.

If you're comfortable with running those optics a bit hotter, you can change the threshold:

```shell
set chassis fpc 1 non-nebs-optics-overheat-trigger 60
```

This is a hidden command, you won't be able to use tab auto-complete here. Note that you can use this command on Junos 15.1.

From what I've seen, most optics are OK with this, and hey, 10Gb optics are cheap enough that it doesn't really matter if you need to replace a few. New ones you buy will probably be NEBS-compliant anyway (or at least report to the system that they are compliant, which is all that really matters...).

## Fix 2: Change the fan speed temperature thresholds

Another approach is to tell the system to kick the fans in a bit earlier. You'll need these hidden commands:

```text
set chassis fpc 2 temperature-threshold fans-to-normal-speed 41
set chassis fpc 2 temperature-threshold fans-on-intermediate-speed 43
set chassis fpc 2 temperature-threshold fans-on-full-speed 48
```

You can play around with different thresholds here. You can probably go a bit higher. This will make the fans kick in at full speed a bit earlier, which will pull down the FPC temperatures. Couple of side effects are screaming fans, and more syslogs complaining about high temperatures. But it lowers temperatures.

These commands do not work with Junos 15.1, so if you have only upgraded one of your REs, you will not be able to do normal commit until both REs are upgraded.

## Fix 3: Use a platform with a better airflow

Part of the problem here is that systems like the MX480 use a dumb cooling design. To quote from the [docs](https://www.juniper.net/documentation/en_US/release-independent/junos/topics/topic-map/mx480-cooling-system.html):

> The air intake to cool the chassis is located on the **side** of the chassis next to the air filter. Air is pulled through the chassis toward the fan tray, where it is exhausted out the **side** of the system. The air intake to cool the power supplies is located in the front of the router above the craft interface. The exhaust for the power supplies is located on the rear bulkhead power supplies.

(_emphasis added_)

This side-to-side airflow is a pain in the ass, and can easily cause overheating problems on its own, regardless of what Juniper is doing with temperature management. Sometimes you need to add these sorts of passive or active cooling units to get better airflow, e.g. the [Vertiv Switchair](https://www.geistglobal.com/switch-air-unit/10664/sa2-006), or similar from [APC](https://www.apc.com/shop/us/en/products/Rack-Side-Air-Distribution-2U-115V-60HZ/P-ACF201BLK). They help, but you end up wasting another 2U, to fix a problem inflicted by your vendor's airflow design decision.
