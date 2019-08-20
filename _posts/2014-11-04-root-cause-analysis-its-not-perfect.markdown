---
author: lindsay
date: 2014-11-04 00:39:32+00:00
layout: post
slug: root-cause-analysis-its-not-perfect
title: Root Cause Analysis - It's Not Perfect
categories:
- NMS
tags:
- NMS
---

Automated Root Cause Analysis promises a lot. High-end network monitoring systems promise that they can automatically isolate network problems, and only tell you about the thing that needs fixing. This sounds very enticing. Who wants a flood of alarms, when we could get just one alarm, telling us what we need to fix? But it's not perfect, and you do need to pay attention to it.

Consider this contrived network:

![RCA Example](/assets/2014/11/RCA-Example.png)

What happens if the upstream link from the router fails?

![RCA Link Down](/assets/2014/11/RCA-Link-Down.png)

From the perspective of the NMS, all systems at that site are unreachable. A simple NMS that is unaware of topology will create 4 alarms - one for each of the router, the switches and the server. A smarter NMS will recognise that it only needs one alarm, for the router WAN link being unreachable (and therefore the whole site is offline). It will know that the switches and server are unreachable, but those alarms will be suppressed by the key incident.

This all sounds like a good idea. Why wouldn't you want that?

But what if the NMS view of the network is incomplete? What might happen then?

Consider the same network as above, but this time a new WAN router has been added, and the old one decommissioned. Now the network looks like this:

![RCA Dual Router](/assets/2014/11/RCA-Dual-Router1.png)

This might be because we've upgraded the old router, and moved to a new WAN link. The old router has now been shut down. What happens if the NMS doesn't discover that new router for some reason - e.g. because it doesn't have SNMP configured properly? Everything seems to tick along, and seems to be working. Traffic will be flowing, and users will think everything is fine. But what if Switch 1 now fails?

No alarms. NMS shows no new incidents. If we drill into the switch, it might show a status change, but it has not raised any new alarms. Why not?

From the perspective of the NMS, it still thinks the path is via the old router. It knows that it can poll the switches and server, but it is still suppressing alerts for those devices, because it thinks the old router is still down. If we were watching a visual map, we would see the colour change for devices, but it won't raise any new incidents.

## How do we fix it?

The key is to pay attention to all incidents in your NMS, and make sure that old incidents are properly resolved. The NMS would still have an older incident, which might have been closed manually. You need to properly investigate incidents, and wherever possible, they should be closed automatically, by the NMS detecting that the issue is now resolved.

You also need to pay close attention to your topologies, and make sure they are correct, and show redundant paths. Finally make sure that new devices do show up in your NMS. Auto-discovery is good, but it's not infallible. Pay attention to these smaller details, and you won't get caught out by a site going offline, and not raising alarms.
