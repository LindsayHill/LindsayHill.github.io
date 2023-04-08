---
author: lindsay
categories:
- Routing &amp; Switching
date: "2023-04-09 10:00 -07:00"
layout: post
slug: enforce-first-as
tags:
- Juniper
title: Enforcing First AS in BGP
---
The BGP RFCs state that external BGP peers should insert their own AS into the AS PATH advertised to eBGP peers. Some peers strip their AS, generally for commercial gain. Juniper and Cisco have opposite default behaviors for handling this. Make sure you set `bgp enforce-first-as` on Juniper routers. Caveats apply.

## Background: Traffic Anomalies

A few years ago I was looking at some traffic reporting anomalies. My IPFIX data said that traffic with next-hop AS \<dodgy-AS\> was around 3Gb. But my SNMP data showed that a PNI to that peer was doing 8-10Gb.

I first doubted my router, because I had issues with IPFIX in the past on that specific platform. I also wondered about sampling rates. I have high flow rates, and need to set the sampling to be more coarse. But it was a big anomaly.

Slicing & dicing the data different ways, and chatting to colleagues about it, we saw what was going on. IPFIX showed the right volumes when reporting on destination interface. But some prefixes received from the peer did *not* contain the peer's AS. We still accepted them.

Huh? Isn't it normal behavior, to insert your own AS into any prefixes you advertise to external peers? It is a key part of BGP loop prevention. Why did my router accept those prefixes? What gives?

## Always Check the RFC

When in doubt, always start with the RFC. They are very readable, and this is exactly the sort of behavior they should define.

[RFC 4271 section 5.1.2](https://www.rfc-editor.org/rfc/rfc4271#section-5.1.2) states that

> When a BGP speaker originates a route then:
>
> a) the originating speaker includes its own AS number in a path segment, of type AS_SEQUENCE, in the AS_PATH attribute of all UPDATE messages sent to an external peer.

Note that there is no "SHOULD", "MAY" or "OPTIONAL" about it.

## Legitimate Exception: Route Servers

Route Servers are a specific, legitimate exception to the above. [RFC 7947 Section 2.2.2.1](https://www.rfc-editor.org/rfc/rfc7947#section-2.2.2.1) states:

> As a route server does not participate in the process of forwarding data between client routers, and because modification of the AS_PATH attribute could affect the route server client BGP Decision Process, the route server SHOULD NOT prepend its own AS number to the AS_PATH segment nor modify the AS_PATH segment in any other way.

Almost all IXPs operate this way today. I peer with a handful that don't, and they annoy me. HKIX is one. PIT Chile changed default behavior this year.

## Why would you strip your AS?

Route servers have a legitimate reason to not insert their AS. Why else would a network do it?

There are other use-cases where you need to manipulate the advertised path, e.g. AS migration. See [Daniel's blog](https://lostintransit.se/2012/08/13/bgp-local-as-command/) for examples.

What about less than legitimate use-cases?

Imagine that I operate a CDN with extensive peering and transit connections.

And let's say that you operate an eyeballs ISP, with two upstream providers. Your upstream providers charge you on a traffic volume basis. They in turn have transit agreements with other operators, and peer at IXPs. They might have bi-lateral peering at IXPs or PNIs with me.

All else being equal, if I have identical relationships with those networks, I will split traffic to you across them. \[Disclaimer: BGP is a suggestion framework, not a proscribed routing protocol like OSPF. I can and do route traffic according to my business needs. Your routing suggestions are just that: suggestions.\]

Now what if one of your transit networks is a bit shady, and wants to maximize traffic going via their network? They have two levers to pull: Either announce more specifics for your prefixes, or strip their own AS. I ignore any other BGP attributes. Announcing more specifics has other issues, and may not be possible. But they can strip their AS, and hope that I don't notice.

Now I have two paths, with everything equal except the AS path length. Default BGP best path selection will make me send traffic via that provider.

Or what if I peer with both you and your transit provider at an IX? I see two paths, with the same AS path length. I will split my traffic between direct to you, and via that transit. That is not cool. If you're at the IX, I should send everything direct to you.

{% include note.html content="AFAIK, this only works with bi-lateral sessions. All route servers drop announcements where the first AS in the path is not the advertising peer's AS." %}

## Juniper vs Cisco behaviour

Juniper and Cisco take different approaches to this. By default, Cisco will only accept prefixes where the first AS matches the eBGP peer AS. You can disable this using the [bgp enforce-first-as](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/command/irg-cr-book/bgp-a1.html#wp1026344430) command.

Juniper *allows* peers to strip their AS by default. You must explicitly set [enforce-first-as](https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/ref/statement/enforce-first-as-edit-protocols.html).

For a typical IXP scenario, if you have a Cisco router, you need to configure "no bgp enforce-first-as" for route server sessions. With a Juniper router you must set "enforce-first-as" for all sessions *except* the route server sessions. There is no equivalent to "no enforce-first-as" on Junos.

## Effects of enforcing first-AS

When enforced, both Cisco and Juniper will discard any prefixes received where the first AS in the path is not the peer's AS. They maintain the BGP session, and they will accept any other valid prefixes received.

Summary: If you peer at IXPs, and use Cisco gear, you're OK. If you're using Juniper, check that your config is enforcing first AS for all sessions except those from route servers.
