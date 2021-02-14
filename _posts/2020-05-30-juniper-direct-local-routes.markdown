---
author: lindsay
date: '2020-05-30 17:30 -07:00'
layout: post
slug: juniper-direct-local-routes
title: Juniper Direct vs Local Routes
categories:
- Routing &amp; Switching
tags:
- Juniper
---

Juniper routers consider a directly configured IP as a "direct" route, except when you use a `/32` mask (for IPv4). Then it is a "local" route. This caused me some confusion when creating a policy to redistribute loopback IP addresses into BGP.

## Route Protocol Types

A router learns routes from a variety of sources - networks configured on the box, those learned from IS-IS, rumors of prefixes from BGP or RIP, etc. You can see the full list [here](https://www.juniper.net/documentation/en_US/junos/topics/reference/command-summary/show-route-protocol.html).

When routes are learned from different sources, Junos uses "[Route Preference Values](https://www.juniper.net/documentation/en_US/junos/topics/reference/general/routing-protocols-default-route-preference-values.html)" to decide which route source to prefer. (Cisco refers to this as [Administrative Distance](https://www.cisco.com/c/en/us/support/docs/ip/border-gateway-protocol-bgp/15986-admin-distance.html)). If routes are otherwise identical, the route with the lowest preference will be installed into the FIB.

If you're looking at the route table, you can narrow down displayed routes to look at a specific type, e.g. `show route protocol direct` to see locally connected networks:

```shell
vagrant@vqfx> show route protocol direct

inet.0: 7 destinations, 7 routes (7 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

10.0.2.0/24        *[Direct/0] 00:02:45
                    >  via em0.0
10.1.2.0/24        *[Direct/0] 00:00:59
                    >  via xe-0/0/0.0
169.254.0.0/24     *[Direct/0] 00:02:49
                    >  via em1.0

inet6.0: 2 destinations, 2 routes (2 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

fe80::205:860f:fc71:8500/128
                   *[Direct/0] 00:02:49
                    >  via lo0.0

{master:0}
vagrant@vqfx>
```

## Route Filtering by Protocol

It's not just about displaying routes, or selecting which route to prefer. We can also use the route type when filtering, to decide which routes we want to redistribute. Let's say we wanted to redistribute static routes (and only static routes) into OSPF. Something like this does the trick:

```shell
[edit]
set policy-options policy-statement export-ospf term statics from protocol static
set policy-options policy-statement export-ospf term statics then accept
set protocols ospf export export-ospf
```

Route filters can get quite complex, matching on all sorts of things - prefix length, route origin, AS path, etc.

So far so good. What if we wanted to write a filter that would match on loopback addresses?

## "Direct" vs "Local"

First a diversion - What's the difference between "Direct" and "Local" routes?

The [docs](https://www.juniper.net/documentation/en_US/junos/topics/reference/command-summary/show-route-protocol.html) say this:

> `direct` — Directly connected route
>  
> ...
>  
> `local` — Local address

OK, so a "direct" route comes from configuring an IP + subnet mask on an interface. If we run `set interface xe-0/0/1 unit 0 family inet address 100.100.100.1/24`, then the router creates a "direct" route for `100.100.100.0/24` via that interface. It will **also** create a `local` entry for `100.100.100.1/32`:

```shell
vagrant@vqfx> show route 100.100.100.0/24

inet.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

100.100.100.0/24   *[Direct/0] 00:00:35
                    >  via xe-0/0/1.0
100.100.100.1/32   *[Local/0] 00:00:35
                       Local via xe-0/0/1.0

{master:0}
vagrant@vqfx>
```

## What About Loopbacks?

What happens when we configure a [loopback interface](https://www.juniper.net/documentation/en_US/junos/topics/concept/interface-security-loopback-understanding.html)? We almost always configure these with a `/32` (or `/128`) subnet mask. How does it show up in the routing table? Is that a "direct" or a "local" route? Should be a "local" route, right? Turns out it's not. It's a **direct** route:

```shell
{master:0}[edit]
vagrant@vqfx# set interfaces lo0 unit 0 family inet address 198.51.100.1/32

{master:0}[edit]
vagrant@vqfx# commit
configuration check succeeds
commit complete

{master:0}[edit]
vagrant@vqfx# run show route 198.51.100.1/32

inet.0: 10 destinations, 10 routes (10 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

198.51.100.1/32    *[Direct/0] 00:00:08
                    >  via lo0.0

{master:0}[edit]
vagrant@vqfx#
```

Hmm. Bit odd. What if we used a different prefix length on the loopback? Now it shows up a little differently:

```shell
{master:0}[edit]
vagrant@vqfx# delete interfaces lo0 unit 0 family inet address 198.51.100.1/32

{master:0}[edit]
vagrant@vqfx# set interfaces lo0 unit 0 family inet address 198.51.100.1/24

{master:0}[edit]
vagrant@vqfx# commit
configuration check succeeds
commit complete

{master:0}[edit]
vagrant@vqfx# run show route 198.51.100.0/24

inet.0: 11 destinations, 11 routes (11 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both

198.51.100.0/24    *[Direct/0] 00:00:05
                    >  via lo0.0
198.51.100.1/32    *[Local/0] 00:00:05
                       Local via lo0.0

{master:0}[edit]
vagrant@vqfx#
```

It must be something to do with the `/32` mask. There's no need to have both a "direct" and a "local" entry for the same prefix, but the choice of "direct" is surprising, to me at least.

## Why Does it Matter?

The reason I noticed this was because I was configuring a policy to redistribute loopbacks into BGP. This was for a leaf-spine network, so I wanted to have the exact same policy configured on all devices. Each system had a `/32` address taken from `198.51.100.0/24`. OK, this should be easy. Let's use this config:

```
set policy-options policy-statement CLOS-OUT term loopbacks from protocol local
set policy-options policy-statement CLOS-OUT term loopbacks from route-filter 198.51.100.0/24 prefix-length-range /32-/32
set policy-options policy-statement CLOS-OUT term loopbacks then accept
set protocols bgp group SPINES export CLOS-OUT
```

Nope. Doesn't work. It _would_ work if I had a shorter mask than `/32` on my loopbacks, but most people aren't going to do that.

The network & prefix length matches, but the protocol doesn't. You have to change it to `from protocol direct`, and then it works.

Funnily enough, if you use something like `test policy CLOS-OUT 198.51.100.1/32`, it will tell you that it accepts the prefix, regardless of whether you use `from protocol local` or `from protocol direct`. But in practice I found it did not export the routes unless I used `from protocol direct`. This was on a recent Junos version. Behavior could be version-specific, I have not tested different versions of Junos.

## No Big Deal. Just Another Gotcha

Ultimately it's no big deal. Just one of those random little things that might confuse someone. If you find this through Google, hope it helps :)
