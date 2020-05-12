---
author: lindsay
date: 2020-05-11
layout: post
slug: juniper-arp-policer
title: Juniper Default ARP Policer
categories:
- Routing &amp; Switching
tags:
- Juniper
---

Juniper devices have a default ARP policer that drops ARP requests and responses over 150kbps. By default, this is an aggregate policer that applies to **all** interfaces. This can lead to unexpected behavior when high levels of ARP on one interface lead to BGP session drops on another interface. You can't change the default policer limits, but you can create a new policer, with higher limits.

## Problem: IPv4 BGP Session Flaps on PNI

I was investigating a problem reported by one of our Transit providers. Once a day or so, our IPv4 BGP session with them would flap. The interface itself was stable, and the IPv6 session remained up. One particular site was seeing this more than others. The sites used different platforms, but were running the same code version.

The curious thing was the logs - we saw log messages saying that we had a notification message saying `NOTIFICATION received from 192.0.2.188 (External AS 64498): code 4 (Hold Timer Expired Error)`. The syslog included this `hold timer 30s, hold timer remain 0s, last sent 2s`. So our router thought it was sending regular KEEPALIVE messages, but the remote end thought it had missed too many.

Looking more closely, we saw that we had BGP session flaps with other neighbors, including iBGP sessions. Clearly it was not a problem with a specific interface, or some other vendor or configuration.

## What's Happening?

Our routers were connected to an IXP that has a large subnet, with many peers. This can mean large amounts of ARP traffic. By default, Juniper routers police ARP to 150kbps. Any ARP requests or responses above that rate are dropped. The interesting wrinkle is that it is not per-interface, but per-PFE.

When the router saw large amounts of ARP traffic on the IX-facing interface, it started policing it. The Transit links we had used the same PFE. During the window when ARP was being policed, it affected ARP on the transit interface. Our router could not get an ARP response for our upstream provider's IP, so it could not send keepalives. The upstream detected loss of keepalives, and sent a notification to us. The session was cleared and reset. By this time we had an ARP entry, and the session quickly came back up.

Of course, IPv6 was unaffected.

## Detecting ARP Policing

There's a couple of things to look at to see if this is affecting you:

```shell
lhill@mx.lab.net> show policer __default_arp_policer__
Policers:
Name                                                Bytes              Packets
__default_arp_policer__                           3091706                67211

{master}
lhill@mx.lab.net>
```

That tells you the aggregate policer has been triggered. To check which FPCs are affected, you need to drill down:

```shell
{master}
lhill@mx.lab.net> start shell pfe network fpc2


NGMPC platform (1200Mhz QorIQ P2020 processor, 3584MB memory, 512KB flash)

NGMPC2(mx.lab.net vty)# show filter index 17000 counters
Filter Counters/Policers:
   Index               Packets                 Bytes  Name
--------  --------------------  --------------------  --------
   17000                 67186                        __default_arp_policer__

NGMPC2(mx.lab.net vty)#
```

I have not seen any syslogs to indicate ARP policing. It is a different framework to the DDoS Protection capabilities.

## Changing ARP Limits

150kbps of ARP traffic is not a huge amount on a router with multiple interfaces in large subnets. So you'll probably want to change the limits.

You can't just change the default ARP limit though. Instead, you have two choices:

1/ Create a new ARP policer, and associate the "busy" interface with that policer. The policer will only apply to that interface, and all the other interfaces will share the default 150kbps.
2/ Create a new ARP policer, and associate interfaces with that. The "busy" interface will be left with the default bucket.

Configuration is pretty simple. First create a new policer:

```
set firewall policer arp_limit if-exceeding bandwidth-limit 1m
set firewall policer arp_limit if-exceeding burst-size-limit 1m
set firewall policer arp_limit then discard
```

Then associate it with an interface:
```
set interfaces ae1 unit 0 family inet policer arp arp_limit
```

## Monitoring the new Policer

You can look at the output of `show policer` - note that if you have created a new policer and associated it with more than one interface, you will see an entry per interface in the `show policer output`:

```shell
lhill@mx.valve.net> show policer ?
Possible completions:
  <[Enter]>            Execute this command
  <policer>            Policer name
  __auto_policer_template_1__
  __auto_policer_template_2__
  __auto_policer_template_3__
  __auto_policer_template_4__
  __auto_policer_template_5__
  __auto_policer_template_6__
  __auto_policer_template_7__
  __auto_policer_template_8__
  __auto_policer_template__
  __default_arp_policer__
  arp_limit-ae1.0-inet-arp
  arp_limit-xe-1/0/1.0-inet-arp
  detail               Show filter statistics with enhanced policer statistics
  logical-system       Name of logical system, or 'all'
  |                    Pipe through a command
{master}
lindsayh@mx.lab.net> show policer arp_limit-ae1.0-inet-arp
Policers:
Name                                                Bytes              Packets
arp_limit-ae1.0-inet-arp                                0                    0

{master}
lhill@mx.lab.net>
```

You can see that I associated the policer with `ae1` and `xe-1/0/1.0`.

Hope this helps someone else looking at flapping BGP or BFD sessions.
