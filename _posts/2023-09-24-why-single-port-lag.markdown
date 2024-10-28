---
author: lindsay
categories:
- Routing &amp; Switching
date: "2023-09-24 15:00 -07:00"
layout: post
slug: why-single-port-lag
tags:
- Juniper
title: Why Single-Port LAGs?
---
I recommend always using LACP for external connections. It will make your life easier, even when you only have a single connection. Here's why we do it.

If you set up a PNI with AS32590, we will strongly recommend the use of LACP, even for a single link. If you have two PNIs with us, they will each be separate single-member LAGs, because they will be on different routers on our side.

It's only once you have more than 2 links that we start using LACP in the way most people think of it.

It's not just us. In Google's [Peering Policy](https://peering.google.com/#/options/peering), under "Private peering physical connection requirements", it states

> Link aggregation via LACP is required for all links, including single links

Ever wondered why that is? What's the point in setting up a LAG if I only have one link? What does it give me? More lines of config for no operational enhancement? And I thought we should use L3 everywhere anyway?

I can't speak for Google, only for the way we operate our network. But I'm pretty sure their reasons are similar to ours. The obvious reason is for future growth, but there are operational benefits too.

## Easy Expansion

Traffic volumes only go one way: up and to the right. If I have an existing 2 x 1 x 100G PNIs to you, and we need more capacity, it's easy - add another port to each LAG. No new IPs needed, no BGP changes.

I can order the cross-connect and patching, and preconfigure my port to be part of the LAG. Then when you patch your end, the new link starts working straight away, no further changes needed.

LAGs can also help when going from _n_ x 10G to _n_ x 100G. `link-speed mixed` seems like the Devil's Work the first time you see it, but it is very useful. Set that option on a 2 x 10G LAG today. When you need to upgrade to 100G, you can add a 100G link to the bundle. It will start balancing traffic, and then you can remove the 10G links, no BGP flaps or changes.

## Operational Benefits

The growth part is obvious. But there's other operational benefits. Consider a real example I am dealing with today. I have a flapping 100G port on a Juniper PTX. The issue only starts with 21.4R3, and only when the remote end is a CFP2 optic, but not all CFP2 optics.

ATAC wants me to move the connection from port 0/0/4 (a QSFP28 port) to 0/0/3 (a QSFP56-DD port). Yes, we are clutching at straws, but this one has been hard to reproduce in the lab, so we're eliminating one more thing.

The router is at a remote site. I need to log a ticket to get remote hands to move the cross-connect. How can I do it with the shortest outage? I'd like to copy the IP address from port 0/0/4 to 0/0/3. That way when the patch cable gets moved, everything comes up:

```bash
lindsayh@ptx# show interfaces et-0/0/4
description "Transit: Potatotel AS64497 [100Gbit]"
unit 0 {
    family inet {
        address 198.51.100.122/31;
    }
    family inet6 {
        address  2001:db8:aa00:15::1/127;
    }
}

{master}[edit]
lindsayh@ptx# copy interfaces et-0/0/4 to et-0/0/3

{master}[edit]
lindsayh@ptx# show | compare
[edit interfaces]
+   et-0/0/3 {
+       unit 0 {
+           family inet {
+               address 198.51.100.122/31;
+           }
+           family inet6 {
+               address  2001:db8:aa00:15::1/127;
+           }
+       }
+   }
```

But I can't do that:

```sh
{master}[edit]
lindsayh@ptx# commit check
re0:
[edit interfaces et-0/0/4 unit 0 family inet]
  'address 198.51.100.122/31'
     identical local address found on rt_inst [default], intfs [et-0/0/3.0 and et-0/0/4.0], family [inet].
error: configuration check-out failed

{master}[edit]
lindsayh@ptx#
```

I need to schedule a time to co-ordinate the move with Equinix, or accept it will be down between when they move it and I'm next online.

If we had used a single-member LAG, it would be easy. Just run `set interfaces et-0/0/3 ether-options 802.3ad ae4`. When they move the cable, everything will work. Later I can clean up the LAG config from the old port.

For your own internal links between devices, you might choose to make them all independent L3 links, and that's OK. That may be the best choice. But any connection to a third party, e.g. PNIs, IXPs, Transit links, you should default to always using LACP, even if you only have one link. A couple of extra lines of config today will save you time later.
