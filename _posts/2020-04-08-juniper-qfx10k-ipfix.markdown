---
author: lindsay
date: 2020-04-08
layout: post
slug: juniper-qfx10k-ipfix
title: Juniper QFX10K IPFIX Gotchas
categories:
- Routing &amp; Switching
tags:
- Juniper
---

IPFIX is problematic on the Juniper [QFX10K](https://www.juniper.net/us/en/products-services/switching/qfx-series/qfx10002/) switches. Documentation is sparse, and doesn't have a complete configuration. Behavior changes between versions in undocumented ways. Here's a couple of things I noticed when upgrading from Junos 17.3 to 17.4. These also apply if you are running 18.4 code. I hit more problems with 18.4, and ended up rolling back to 17.4.

## Big Changes in Reported Throughput

Here's a graph showing total reported throughput for a QFX10K I upgraded:

[![ipfix traffic report](/assets/2020/04/ipfix_traffic.png)](/assets/2020/04/ipfix_traffic.png)


There's a few things going on there. First the reported traffic drops to zero after I upgraded. Then it starts coming up, after I fixed the first problem. But then after that the reported traffic is flat, and lower than it should be. Then it starts coming up again after I made the second fix.

## First Problem: Chassis Sample Instance

The first configuration change I needed to add was this: `set chassis fpc 0 sampling-instance sample-border`, where `sample-border` is the name of the sampling instance I have configured under `forwarding-options`. This was not required with 17.3. If you don't do it with 17.4, you won't get any data.

## Second Problem: DDoS-Protection

Some Juniper platforms implement [Control Plane DDoS Protection](https://www.juniper.net/documentation/en_US/junos/topics/concept/subscriber-management-ddos-protection.html). Implementation varies across platforms and versions. Generally it works OK, but can be very confusing, especially with systems that have shared policers. It might say one thing is triggering a violation, but it's actually something else.

Turns out that in this case something changed between 17.3 and 17.4, and high volumes of IPFIX now trigger ddos-protection violations. The system ends up throttling itself.

I had to make these changes:

```text
set system ddos-protection protocols resolve aggregate bandwidth 8000
set system ddos-protection protocols exceptions aggregate bandwidth 4000
set system ddos-protection protocols ndpv6 aggregate bandwidth 8000
```

It's not obvious why I need to change the `resolve` or `ndpv6` settings. My guess is that it's something to do with shared policers.

If you have low levels of traffic, you won't hit these limits. My guess is that IPFIX testing for this platform was in low-bandwidth labs, and so they didn't hit the limits. 

## Still Don't Quite Trust It

Reporting is still a bit flaky for these systems, and IPFIX numbers don't quite match up to interface statistics. Treat IPFIX as more of a hint on the QFX10K, rather than something to rely on.