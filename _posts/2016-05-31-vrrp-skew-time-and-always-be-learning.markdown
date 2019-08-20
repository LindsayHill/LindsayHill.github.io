---
author: lindsay
date: 2016-05-31 04:53:10+00:00
layout: post
slug: vrrp-skew-time-and-always-be-learning
title: VRRP Skew Time (and always be learning...)
categories:
- Routing &amp; Switching
tags:
- networking
---

It's funny how you can work with something for years, but miss a small detail. This week I learnt about Skew Time for VRRP. The reason for it is completely obvious once you think about it, but for some reason the detail had escaped me for all these years.

## VRRP Hellos

VRRP sends out a "hello" multicast every `<hello>` seconds. Usually this is something like every 1 or 3 seconds. Unlike HSRP, only the current master sends out hello messages. This contains the current master priority & status.

The backup devices listen out for this hello message. If they think they have a higher priority, or if they fail to hear the hello message, they will assume the role of master.

## Down Interval

Changing from backup to master because of one missed hello could cause network instability. There's a common rule used for all keepalive-type messages, where backup devices will wait for three missed polls/keepalives before declaring something 'down.'

{% include note.html content="NB: HSRP is slightly different here - the holdtime can be manually specified, including to a shorter time than the hello time, if you're feeling spectacularly stupid." %}

VRRP is similar. It waits three poll intervals before declaring the master 'down,' and attempting to take over. Simple, right? At least that's what I always thought. Turns out there's an additional delay called 'skew time.'

## Skew Time

If you run "show vrrp" on a Cisco router, you'll see output like this:

```bash
Router# show vrrp
Ethernet0/1 - Group 1
State is Master
Virtual IP address is 10.21.0.10
Virtual MAC address is 0000.5e00.0101
Advertisement interval is 1.000 sec
Preemption is enabled
 min delay is 0.000 sec
Priority is 100
  Authentication MD5, key-string, timeout 30 secs
Master Router is 10.21.0.1 (local), priority is 100
Master Advertisement interval is 1.000 sec
Master Down interval is 3.609 sec

```

(Above from [Cisco Docs](http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipapp_fhrp/configuration/15-mt/fhp-15-mt-book/fhp-vrrp.html))
Note the "Master Down interval" there - it's not 3s, but 3.609s.

Turns out that a "skew time" is added to the down interval. Cisco devices calculate Skew time as (256-priority)/256. Brocade VDX switches take a more simplistic approach - they calculate skew time as "If priority > 128, then skew time = 0, otherwise skew time = 1."

## Why do we need skew time?

I was used to working with networks with two devices - e.g. a pair of IPSO firewalls. What's the reason for skew time if you have a pair of devices? Why do you want to insert a seemingly random delay?

It makes more sense when you have more than 2 devices in a VRRP group. If the current master goes offline, then you will have multiple devices attempting to assume the master role simultaneously. If you have configured priorities appropriately then it will all settle down (once the lower priority devices realise there is a higher priority device operating). But there will be some instability in the meantime - remember that only the current master sends hellos, so the backup routers don't know if there are any other backup routers on the network. Better to add a small priority-based delay, so that it's more likely that the next-highest priority backup device will become master.

All very clever. I like it when I find these small details, and realise that the protocol writers are fairly clever after all.
