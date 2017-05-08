---
author: lindsay
date: 2015-05-12 20:56:55+00:00
layout: post
slug: f5-apm-srx-and-dtls-nat-timeout
title: F5 APM, SRX and DTLS NAT Timeout
categories:
- Security
tags:
- F5
- SRX
---

I have been having issues using the [F5 APM](https://f5.com/products/modules/access-policy-manager) client behind a Juniper [SRX-110](http://www.juniper.net/us/en/products-services/security/srx-series/srx110/) using hide NAT. I believe I've tracked it down to the default timeout settings used for UDP services. Here's what I did to resolve it.



## Constant Connection Timeouts



The laptop client was behind the SRX-110, using hide NAT. The initial client connection would work, and things would look good for a while. The the client would stop receiving packets. Traffic graphs would show a little bit of outbound traffic, and nothing inbound. Eventually, the client might decide it needed to reconnect. But usually, it would sit there for a few minutes doing nothing. Then I would force a disconnect, which would take a while, and then reconnect. Exceedingly frustrating.

Connecting the client to a different network - e.g. using a phone hotspot - worked fine. No dropouts. Using a wired connection behind the SRX had the same issue. So clearly the problem was related to the SRX.



## TLS & DTLS



I dug into the traffic flows to better understand what was going on. This SSL VPN solution makes an initial TLS connection using TCP 443. It then switches over to DTLS using UDP 4433 for ongoing encrypted traffic streams.

The problem seemed to be related to the default UDP service NAT timeout on the SRX. This is 60s. TCP sessions have a default timeout of 1800s.

So if I didn't have any encrypted traffic for 60s, the DTLS NAT session would time out. But the client still had TCP 443 connectivity. It then seemed to get into a confused state, where it didn't fully recognise that the VPN wasn't working properly. That's why it would take so long to time out, and needed a forced disconnect.



## Extended Timeouts



I made a couple of small changes to my SRX configuration. I defined a new DTLS application, with a NAT timeout of 600s:


```text
lhill@srx01> show configuration applications 
application dtls {
    protocol udp;
    destination-port 4433;
    inactivity-timeout 600;
}

lhill@srx01> show configuration applications | display set 
set applications application dtls protocol udp
set applications application dtls destination-port 4433
set applications application dtls inactivity-timeout 600
```


I have a basic security policy that allows everything outbound. It contained this:


```text
lhill@srx01> show configuration security policies from-zone trust to-zone untrust 
policy trust-to-untrust {
    match {
        source-address any;
        destination-address any;
        application any;
    }
    then {
        permit;
        log {
            session-close;
        }
        count;
    }
}

lhill@srx01> show configuration security policies from-zone trust to-zone untrust | display set 
set security policies from-zone trust to-zone untrust policy trust-to-untrust match source-address any
set security policies from-zone trust to-zone untrust policy trust-to-untrust match destination-address any
set security policies from-zone trust to-zone untrust policy trust-to-untrust match application any
set security policies from-zone trust to-zone untrust policy trust-to-untrust then permit
set security policies from-zone trust to-zone untrust policy trust-to-untrust then log session-close
set security policies from-zone trust to-zone untrust policy trust-to-untrust then count
```


I added a new clause, above the existing policy, to ensure that my application was matched:


```text
lhill@srx01> show configuration security policies from-zone trust to-zone untrust policy dtls      
match {
    source-address any;
    destination-address any;
    application dtls;
}
then {
    permit;
}

lhill@srx01> show configuration security policies from-zone trust to-zone untrust policy dtls | display set
set security policies from-zone trust to-zone untrust policy dtls match source-address any
set security policies from-zone trust to-zone untrust policy dtls match destination-address any
set security policies from-zone trust to-zone untrust policy dtls match application dtls
set security policies from-zone trust to-zone untrust policy dtls then permit
```


Now that policy gets hit for DTLS traffic. Checking the flows with `show security flow session nat destination-port 4433`, I can see the longer timeouts applied.

Best of all, no more dropouts!
