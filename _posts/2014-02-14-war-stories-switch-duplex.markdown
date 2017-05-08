---
author: lindsay
date: 2014-02-14 05:00:36+00:00
layout: post
slug: war-stories-switch-duplex
title: 'War Stories: Switches Lying about Duplex Mismatches'
categories:
- Routing &amp; Switching
series: 'War Stories'
tags:
- war stories
---

{% include series.html %}

Continuing our intermittent series of networking war stories, this time it's switches lying about duplex mismatches. All Network Engineers are aware of duplex mismatches, and the havoc they can cause. We know the signs to look for, and how to resolve it. But what if IOS is lying to us, and not showing the usual tell-tale signs?


## Duplex Mismatches


From [Wikipedia](http://en.wikipedia.org/wiki/Duplex_mismatch):

> a duplex mismatch is a condition where two connected devices operate in different duplex modes, that is, one operates in half duplex while the other one operates in full duplex. The effect of a duplex mismatch is a link that operates inefficiently.

If you've seen a duplex mismatch in action, you'll see that the link comes up, but data transfer rates are poor - typically something like 10Mb/s. People don't always pick it up straight away, as basic connectivity tests succeed. You're busy, pings work, move on...until people start complaining about link performance.

There's a couple of different scenarios where a duplex mismatch can occur. The most common case is where the speed & duplex has been fixed on end, and the other is set to auto/auto. The system that is fixed doesn't enter the negotiation process, so the auto/auto end assumes it must be connected to a dumb hub that can't negotiate, so it falls back to 100/half. There is also some evidence of problems between specific NICs and switch models, but these days that is extremely unusual.

Normally we can quickly identify a mismatch by looking at the interface counters, using `show interface f_x/y_`. If the switch is operating at full-duplex, but the remote end is half-duplex, we will see input CRC errors. The half-duplex end will see output errors - usually late collisions.


## Why can't I get more than 10Mb/s on this link?


I worked for an organisation that had a policy of hard-coding speed & duplex on all network ports that had servers or other network gear attached. This worked well when it was a switch connecting to a router, less so when plugging in servers, because they were administered by another team. But we were quick at spotting the signs of a half-duplex server, and we knew exactly how to resolve it.

We installed some new network infrastructure to support Internet access via a proxy. New firewalls, and the latest & greatest L2 switch available - the 2950. I was still a young engineer at the time, so I'd just installed the switches with whatever IOS version they shipped with. In this case it was 12.1(13)EA1.

All connectivity looked good, and Internet access via proxy seemed to be working. We only had a 10Mb Internet link, so I hadn't done any serious performance testing. We switched over Internet access for staff, and everything seemed to be OK.

It was only later that I was transferring some data directly to the firewall, and I realised performance was shocking. Hmm....I went through all the steps in the network path, evaluating all the links. Through a process of elimination, I narrowed it down to the new 2950 switches. They were the source of the poor performance, but it didn't make sense: The firewall interface was set to 100/full, and so was the switchport. Everything was hard-set, so why was performance poor?

Looking at the switchport interfaces, I saw this sort of output:


```text
Switch#sh int f0/5
FastEthernet0/5 is up, line protocol is up (connected)
  ...snip...
Full-duplex, 100Mb/s
  ...snip...
   61654 packets input, 10428868 bytes, 0 no buffer
     Received 15018 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog, 3706 multicast, 0 pause input
     0 input packets with dribble condition detected
     48819 packets output, 23500665 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 babbles, 0 late collision, 1736 deferred
     0 lost carrier, 0 no carrier, 0 PAUSE output
     0 output buffer failures, 0 output buffers swapped out
```


So it's saying that it's running at 100/full, and that there's no CRC errors. What's going on?


## Because the switch is lying


The key is in that highlighted line above: a non-zero rate of "Deferred" packets. I spent a lot of time trying to figure out what this meant, and what was causing it. At the time there wasn't much information available. If you search for it these days, the top 10 Google results for "deferred packets" will tell you - it's this bug: [CSCea56745](https://tools.cisco.com/bugsearch/bug/CSCea56745)

> When interface configured its duplex as full, deferred counter increments, then packets through that link are delayed.
> 
> A device affected by this DDTS may also not account for dropped packets correctly resulting in lower throughput and performance.

If you set both ends to auto/auto, it would work perfectly. But if you tried hard-coding both ends, you'll get poor performance. Frustratingly, you won't get the right counters to tell you what's going on though. The solution is to upgrade to 12.1(13)EA1b or later, or use auto/auto as a workaround.

Of course, you might be asking yourself: "How the hell could Cisco ship that IOS as a default if it couldn't even do something as simple as full-duplex switching? I understand obscure features not working, but core switching? Really?"


## New Site, Same Problem


A few years later I came across something that looked similar - a new server was getting shocking transfer rates if the speed & duplex were locked to 100/full on both server & switch.  The network team were trying to tell me that no, it wasn't a duplex mismatch, they weren't seeing any CRC errors on their side. I couldn't check their configuration, as I had no access to the network gear. But I suspected it could be a similar situation to what I'd seen before - so I captured some CDP traffic, which told me the switch model & IOS version. I could then give that information to the network team, along with the Cisco Bug ID, to prove it. They finally accepted it, but they weren't going to upgrade the IOS anyway. No problem, this time I knew the workaround: Set both ends to auto. It's always a lot faster fixing the problems the second time around...


## Use Auto/Auto Everywhere


These days I always set everything to auto, unless I have an outstandingly good reason for locking it. Gigabit networks [basically require the use of auto/auto](http://etherealmind.com/ethernet-autonegotiation-works-why-how-standard-should-be-set/). Make sure you've moved with the times, and use auto/auto as your standard practice too. Failure rates with Gigabit and auto/auto are vanishingly small - and you're not running 100Mb networks anymore, right?

The other moral of the story is: Don't just blindly run gear with whatever firmware it ships with - check it, and upgrade/downgrade if required.
