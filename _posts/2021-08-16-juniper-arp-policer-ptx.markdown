---
author: lindsay
date: '2021-08-16 23:00 -07:00'
layout: post
slug: juniper-arp-policer-ptx
title: Juniper ARP Policer on PTX
categories:
- Routing &amp; Switching
tags:
- Juniper
---

I've written before about the [default ARP policer on Juniper MX]({% post_url 2020-05-11-juniper-arp-policer %}). It can create some odd failure conditions when you're connected to noisy networks such as large Internet Exchanges. [Junos OS Evolved](https://www.juniper.net/documentation/us/en/software/junos/evo-overview/topics/concept/evo-overview.html), as used on platforms like the [PTX10003](https://www.juniper.net/us/en/products/routers/ptx-series/ptx10003-packet-transport-router.html) has low default values for ARP and ICMPv6 ND DDoS protections. It will cause the same problems, but is easier to diagnose and mitigate.

## Juniper DDoS Protection

Platforms like MX, QFX, PTX have [Control Plane DDoS protections](https://www.juniper.net/documentation/us/en/software/junos/security-services/topics/concept/subscriber-management-ddos-protection.html) built in. These will automatically rate-limit various traffic types that hit the CPU. This is generally a Good Thing. Certain packet types get punted from the ASIC to the CPU, but the CPU can't handle anywhere near the traffic levels that the forwarding ASIC can. Send enough special packets to a router, choke the CPU, and you might be able to knock things offline. So having default policies to rate-limit traffic makes sense.

## Platform Defaults

Juniper might have "One Junos" but we know it's not that simple. Behavior varies between platforms. Check these default values for some DDoS protections for different platforms:


|Protocol  | MX       | QFX      |PTX       |
|----------|----------|----------|----------|
|ARP|20,000|500|500|
|NDPv6|20,000|N/A|500|
|ICMP|20,000|N/A|500|
|BGP|20,000|3,000|5,000|


Note how the PTX values are much closer to the QFX values than the MX.

## Diagnosis and Mitigation

Those PTX ARP & NDPv6 values will cause you problems on a busy IX. This behavior shows up as flapping BGP sessions, especially IPv6 BGP sessions. It can be confusing at first, as you appear to have working connectivity. Most peers are unaffected. You might not pick up on it if you're not looking at your logs. Exact symptoms will vary, and you will see some neighbors flap more than others.

`show ddos-protection violations` will show currently violating ddos-protections.

Run `show ddos-protection protocols ndpv6` to see current traffic values, if/when it last triggered, and the number of times it has triggered.

Check your syslog server for DDOS_PROTOCOL_VIOLATION_SET entries.

Raising the limits is easy:

```
lindsayh@ptx> show configuration system ddos-protection
protocols {
    arp {
        aggregate {
            bandwidth 8000;
            burst 8000;
        }
    }
    ndpv6 {
        aggregate {
            bandwidth 8000;
            burst 8000;
        }
    }
}

lindsayh@ptx>
```

If you have an active violation, it's useful to run `clear ddos-protection protocols states` after making changes. Otherwise you have to wait a bit longer for the timer to expire.

## But Why so Low?

The PTX platform started life as a high-bandwidth, low-featureset device. Typical use-case was an LSR, where you have P2P links, and low levels of ARP traffic. Picking 500 pps was a reasonable default.

But the PTX featureset has evolved, and now it's suitable for edge peering. For 100G/400G platforms, the price is much better than MX. So people are starting to deploy it in new places in the network. Being on the leading edge means you'll hit a few rough edges, bugs, or in this case simply defaults that don't make sense.

No big deal. I expect that Juniper will change these defaults in the near future. With luck, this will be resolved before you ever hit it.
