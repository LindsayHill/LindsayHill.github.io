---
author: lindsay
date: 2015-07-15 03:03:15+00:00
layout: post
slug: ipv6-test-com-and-srx-firewall-policies
title: IPv6-test.com and SRX firewall policies
categories:
- Security
tags:
- IPv6
- SRX
---

[ipv6-test.com](http://ipv6-test.com/) is a useful site for testing IPv4 & IPv6 connectivity. It checks that v4 & v6 are working as expected, and reports your browser v4/v6 preferences. It does have one oddity with ICMPv6 tests. Here's what I did to work around it with my SRX setup.

The site runs a suite of tests and gives you a score out of 20. Most dual-stack home users will probably get 17/20. They deduct 1 point for no reverse DNS entry for v6, and 2 points for "ICMP Filtered"

[![icmp-test-fail](/assets/2015/07/icmp-test-fail.jpg)](/assets/2015/07/icmp-test-fail.jpg)

> How can you improve your score ?
>
> 1. Reconfigure your firewall
> Your router or firewall is filtering ICMPv6 messages sent to your computer. An IPv6 host that cannot receive ICMP messages may encounter problems like some web pages loading partially or not at all.
>
> 2. Get a reverse DNS record

The first one is fine, but the second issue is a worry. [ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol_version_6) is a critical part of IPv6. It's needed for things like Neighbor Discovery, and [Packet Too Big](https://en.wikipedia.org/wiki/IPv6_packet#Fragmentation) messages.

Most home user firewall setups will be fairly simple. Basically 'Allow everything out, and allow related traffic back in. Drop everything else.' Surely the default policy on the SRX should be allowing related ICMPv6 messages back in? Let's check some tcpdump output on our client:

```sh
~ lhill$ sudo tcpdump -ni en0 icmp6 and not net fe80::/10
Password:
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on en0, link-type EN10MB (Ethernet), capture size 65535 bytes
11:38:04.398471 IP6 2001:41d0::b1c > 2406:e007:aaaa:1111:cccc:dddd:eeee:e0ce: ICMP6, destination unreachable, unreachable address 2001:41d0:8:e8ad::fa11:bac, length 92
```

We can see the destination unreachable message returned. So why does the test fail?

It's because that page is just doing a simple echo-request. It assumes that if it can ping you, then all ICMPv6 must be allowed. This is wrong, but it was probably the simplest test to code.

If you want that test to pass, you'll need to make some changes to your local firewall. Here's what I added to my SRX:

```text
lhill@srx01# set security policies from-zone untrust to-zone trust policy icmpv6 match source-address any-ipv6 destination-address any-ipv6 application junos-icmp6-echo-request

[edit]
lhill@srx01# set security policies from-zone untrust to-zone trust policy icmpv6 then permit
lhill@srx01# show security policies from-zone untrust to-zone trust
policy icmpv6 {
    match {
        source-address any-ipv6;
        destination-address any-ipv6;
        application junos-icmp6-echo-request;
    }
    then {
        permit;
    }
}

[edit]
lhill@srx01#
```

My trusted zone is the internal network, and untrusted is outside the WAN interface. I didn't have any policy in place for that direction. You can see I've added a new policy that matches the ICMPv6 echo-request. Now reload the IPv6-test page:

[![icmp-test-pass](/assets/2015/07/icmp-test-pass.jpg)](/assets/2015/07/icmp-test-pass.jpg)

And now check the tcpdump output:

```shell
~ lhill$ sudo tcpdump -ni en0 icmp6 and not net fe80::/10
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on en0, link-type EN10MB (Ethernet), capture size 65535 bytes
11:38:03.872779 IP6 2001:41d0:8:e8ad::1 > 2406:e007:aaaa:1111:cccc:dddd:eeee:e0ce: ICMP6, echo request, seq 0, length 64
11:38:03.872865 IP6 2406:e007:aaaa:1111:cccc:dddd:eeee:e0ce > 2001:41d0:8:e8ad::1: ICMP6, echo reply, seq 0, length 64
11:38:04.398471 IP6 2001:41d0::b1c > 2406:e007:aaaa:1111:cccc:dddd:eeee:e0ce: ICMP6, destination unreachable, unreachable address 2001:41d0:8:e8ad::fa11:bac, length 92
```

Now we can see the echo request/reply traffic.

No real difference to usability, but hey, now that test passes.
