---
author: lindsay
date: 2014-01-15 03:44:29+00:00
layout: post
slug: dhcpv6-juniper-progress
title: DHCPv6 on Juniper SRX-110 - Progress
categories:
- Routing &amp; Switching
tags:
- IPv6
- juniper
---

Last year I posted about my frustrations with getting the [DHCPv6 client working]({% post_url 2013-08-07-dhcpv6-client-srx %}) on a Juniper SRX-110. I am pleased to report that Juniper has now released [12.1X46-D10.2](http://www.juniper.net/support/downloads/?p=srx110#sw), which resolves at least some of my issues. I couldn't find it documented in the [Release Notes](http://www.juniper.net/techpubs/en_US/junos12.1x46/information-products/topic-collections/release-notes/12.1x46/index.html) anywhere, but the parser now allows you to have `client-ia-type ia-pd` without also requiring `client-ia-type ia-na`

```text
root@srx01> show configuration interfaces at-1/0/0 unit 0 family inet6 dhcpv6-client
client-type statefull;
client-ia-type ia-pd;
rapid-commit;
update-router-advertisement {
    interface vlan.0;
}
client-identifier duid-type duid-ll;
req-option domain;
req-option dns-server;
update-server;

root@srx01>
```

It might not seem like much, but this is real progress for me - now my router can successfully reboot, without needing either a console cable, or resetting to a rescue configuration.

I haven't yet had a chance to try getting the SRX to pass DHCPv6 prefixes from an external DHCPv6-client interface to the DHCPv6 server operating on an internal interface. In theory it should work, but I haven't seen a complete working config. I don't have an urgent need to fix that, as I have it working with RAs.
