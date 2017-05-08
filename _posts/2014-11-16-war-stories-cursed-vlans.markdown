---
author: lindsay
date: 2014-11-16 17:00:59+00:00
layout: post
slug: war-stories-cursed-vlans
title: 'War Stories: Cursed VLANs'
categories:
- Routing &amp; Switching
series: 'War Stories'
tags:
- switching
- war stories
---

{% include series.html %}

I’ve written before about switch ports being [permanently disabled]({% post_url 2014-02-03-war-stories-loop-network-meltdown %}). This time it’s something new to me: VLANs that refuse to forward frames.


## A Simple Network


The network was pretty straightforward. A pair of firewalls connecting through a pair of switches to a pair of routers:

[![Cursed VLAN](/assets/2014/11/Cursed-VLAN.png)](/assets/2014/11/Cursed-VLAN.png)

Sub-interfaces were used on the routers and firewalls, with trunks to the switches. VLAN 100 was used for 100.100.100.0/24, and VLAN 200 was used for 200.200.200.0/24. The switches were configured to pass VLANs 100 & 200.

All was working as expected. All devices could see each other on all VLANs.


## Until it stopped


We received reports that we’d lost reachability to Router A’s VLAN 200 sub-interface. After doing some investigation, we could see that Firewall-A could no longer see Router A’s MAC address on G0.200. But everything else was fine - the VLAN 100 interface worked perfectly. So we knew it couldn’t be a physical interface issue.

Hmmm. What’s going on? First instinct: check the switch port configuration. Has anything changed? Nope. VLAN 200 still there, configured as expected. The router & firewall were still tagging frames with VLAN 200. But they couldn’t see each other, and the switch wasn’t learning any MAC addresses on that VLAN.

Spanning-tree? Nope, all ports in forwarding state. VLAN still there in the VLAN database? Yep, looks OK. What about Firewall-B and Router-B? They can see each other, but they can’t see Firewall-A or Router-A. Switch-2 shows MAC addresses for Firewall-B and Router-B, but nothing on the link to Switch-1.

Why didn’t it work?


## Workaround: Move to a new VLAN


We re-configured the Firewall & Router sub-interfaces to use VLAN 300 instead. We added that VLAN to the associated trunk ports, and everything started working. Full connectivity restored. But VLAN 200? It seems to be cursed.

I still haven’t figured out what happened here. Anyone ever seen anything like this?
