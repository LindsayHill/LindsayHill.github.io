---
author: lindsay
date: 2013-08-07 02:06:21+00:00
layout: post
slug: dhcpv6-client-srx
title: DHCPv6 Client on Juniper SRX-110
categories:
- Routing &amp; Switching
tags:
- IPv6
- juniper
- SRX
---

Juniper recently released [12.1X45-D10 for the SRX-110](http://www.juniper.net/techpubs/en_US/junos12.1x45/information-products/topic-collections/release-notes/12.1x45/index.html). The key new feature for me was [DHCPv6 Client](http://www.juniper.net/techpubs/en_US/junos12.1x45/information-products/topic-collections/release-notes/12.1x45/index.html?topic-72762.html#jd0e458) support - finally! It's still new, and buggy, and I wouldn't rush it into production, but if you need DHCPv6 client support, it's here. These are my notes on how I've configured it to get it working.

**Background:** The SRX-110 is a small ADSL router/firewall, aimed at the SOHO market. 8x100Mbit ports, and one ADSL2+/VDSL POTS interface. Earlier this year I purchased one, to help with learning JunOS. It's a work in progress. I run it as my primary router at home, so I do have to be careful about any self-induced Internet outages. Wife SLA can be very demanding.

My [ISP](http://www.snap.net.nz/) offers native IPv6 connectivity by default to all customers. Not many do. So I thought I would be in great shape with the SRX - I would be able to get up and running with this IPv6 thing. So far all my IPv6 experience has been in books and labs. Time to make it real! Nope. Not so fast. SRX didn't support DHCPv6 client. Supported pretty much every other IPv6 feature, but not that. A DHCPv6 client is needed for Prefix Delegation (PD). This is used by ISPs to allocate a /48 IPv6 prefix to a CPE. When your modem connects to the ISP, as part of the negotiations, it sends an IA_PD request, which the ISP responds to with a prefix delegation. It will then route to that /48 via the CPE. The CPE can then decide how to split that /48 up, and allocate it to internal clients. You might have say several /64 prefixes you use internally.

There's a few moving parts here. One is that the CPE needs to request, and receive a prefix. This is the DHCPv6 client part. It then needs to have a way to break up that prefix, and tell internal clients what prefix to use. The SRX can either use RA (Router Advertisements), or it can act as a DHCPv6 server, to allocate addresses to internal clients. So we've got a device acting as both a DHCPv6 client and a DHCPv6 server. Obviously there also needs to be a mechanism to pass the received prefixes from the client to the server. 12.1X45-D10 promised to deliver all this functionality.

First, let's look at the relevant bits of the config:

```text
root@srx01> show configuration interfaces at-1/0/0 unit 0 family inet6 dhcpv6-client
client-type statefull;
##
## Warning: IA-NA identity association is required for ia-pd
##
client-ia-type ia-pd;
rapid-commit;
client-identifier duid-type duid-ll;

root@srx01> show configuration security forwarding-options
family {
inet6 {
mode flow-based;
}
}

root@srx01> show configuration routing-options
rib inet6.0 {
static {
route ::/0 next-hop at-1/0/0.0;
}
}
static {
route 0.0.0.0/0 next-hop at-1/0/0.0;
}

root@srx01> show configuration security zones security-zone untrust
screen untrust-screen;
interfaces {
at-1/0/0.0 {
host-inbound-traffic {
system-services {
dhcpv6;
}
}
}
}

root@srx01>
```

Couple of things to note here: Firstly, note the warning about not enabling "client-ia-type ia-na". At first, I was unable to get the config to commit without that option. While it now lets me commit that config, I have heard that it may not successfully reboot. Yikes. The problem was that if I did enable it, then my DHCPv6 client session never completed, because my ISP does not hand out IA_NA responses. So the SRX would stay stuck in INIT:

```text
root@srx01> show dhcpv6 client binding detail

Client Interface: at-1/0/0.0
     Hardware Address:             54:e0:32:d0:b3:3f
     State:                        INIT(DHCPV6_CLIENT_STATE_INIT)
     ClientType:                   STATEFULL
     Bind Type:                    IA_NA IA_PD
     Client DUID:                  LL0x1-54:e0:32:d0:b3:3f
     Rapid Commit:                 On
     Server Ip Address:            ::/0
     Update Server                 Yes
     Client IP Address:            ::/0
     Client IP Prefix:             2406:e000:b33f::/48

root@srx01>
```

Support suggested using `client-type autoconfig` but you must use `client-type statefull` if you want to do PD. Probably the other key point here is ensuring that you allow inbound dhcpv6. Otherwise it won't get anywhere. Now, we can see the prefix successfully delegated to us:

```text
root@srx01> show dhcpv6 client binding detail

Client Interface: at-1/0/0.0
Hardware Address: 54:e0:32:d0:b3:3f
State: BOUND(DHCPV6_CLIENT_STATE_BOUND)
ClientType: STATEFULL
Lease Expires: 2013-08-08 05:17:36 NZST
Lease Expires in: 55301 seconds
Lease Start: 2013-08-07 05:17:36 NZST
Bind Type: IA_PD
Client DUID: LL0x1-54:e0:32:d0:b3:ef
Rapid Commit: On
Server Ip Address: ::
Update Server Yes
Client IP Prefix: 2406:e000:b33f::/48

DHCP options:
Name: server-identifier, Value: VENDOR0x00000a4c-0x4531b33f

root@srx01>
```

OK, so how do we hand that out to internal clients?  Here's what I've done, using RA:

```text
set interfaces at-1/0/0 unit 0 family inet6 dhcpv6-client update-router-advertisement interface vlan.0
```

That tells the SRX to advertise the received prefix out vlan.0, using RAs. Note that if I had any configuration at all under `protocols router-advertisement` the configuration would not pass commit check. Let's take a look at my Mac, and see if it's worked:

```bash
lhill$ ifconfig en0
en0: flags=8863<up,broadcast,smart,running,simplex,multicast> mtu 1500
ether 04:0c:ce:99:99:99
inet6 fe80::60c:ceff:fe99:9999%en0 prefixlen 64 scopeid 0x4
inet6 2406:e000:5555:1:60c:ceff:fee2:9999 prefixlen 64 autoconf
inet6 2406:e000:5555:1:1117:ed56:d253:b33f prefixlen 64 autoconf temporary
inet 192.168.100.100 netmask 0xffffff00 broadcast 192.168.100.255
media: autoselect
status: active
```

So it works! With that setup, I'm able to access IPv6-only content. Running Wireshark and looking for IPv6 traffic, sadly most of it seems to be web banner ads. But it's a start. I would like to make this work with the DHCPv6 server functionality on the SRX, but I have been unable to get it working. The clients send DHCPv6 SOLICIT messages, but the SRX drops them all pre-authentication, without really telling me WHY it drops them. I am still dealing with support on this. I will post a follow-up if I can get that working.

**[UPDATE 2013-01-15]** Juniper released 12.1X46-D10.2 in December 2013. This now allows `client-ia-type ia-pd` without requiring `client-ia-type ia-na`. See more [here]({% post_url 2014-01-15-dhcpv6-juniper-progress %}).
