---
author: lindsay
date: 2013-07-29 19:00:59+00:00
layout: post
slug: managing-hybrid-networks
title: Managing Hybrid Networks
categories:
- HP IMC
tags:
- hybrid
- imc
- OpenFlow
- SDN
---

The most interesting talk I attended at HP Discover in Las Vegas this year was Ken Gott's talk on "[Management of Software Defined Networks and Hybrid Environments with HP IMC](http://h30614.www3.hp.com/Discover/OnDemand/LasVegas2013/SessionDetail/c0f06c65-e4ec-4f64-9be5-1aed2d396786#.UfOSVGRga2w)." This talk outlined the architecture for how HP plans to manage hybrid network environments using IMC, and gave us a quick demo of the code.

This is the HP Marketing view of the overall architecture:

[![HP's SDN Architecture. ](/assets/2013/07/sdn-architecture.jpg)](http://h17007.www1.hp.com/us/en/networking/solutions/technology/sdn/index.aspx#tab=TAB1)

{:.image-caption}
HP's SDN Architecture.

SDN applications, such as HP's Sentinel, or Lync integration, will communicate with the SDN controller, using APIs. The SDN controller will use OpenFlow to push forwarding rules to the underlying network infrastructure. IMC sits along the right-hand side, talking at all levels to the other components. It communicates with the applications running on the SDN controller, the SDN controller itself, and the underlying infrastructure. It can be used to close the feedback loop from the network, using real-time network data to tell the SDN controller to change forwarding decisions.

HP hasn't published the slides from the presentation, but here's my recollection of how the layers communicate:

[![IMC SDN Interaction](/assets/2013/07/IMC_SDN.png)](/assets/2013/07/IMC_SDN.png)

{:.image-caption}
It's all about the API

Note that the other applications on the right could be anything - these could be other HP-developed systems, or they could be custom-developed.


## Key Features


HP talked about some interesting features that they plan on releasing:


  * All network devices, showing topologies, configuration, performance and faults in one place - whether OpenFlow-enabled or not.

  * Flow-based Topology view of the network in IMC - choose to see paths across network based on flow. This is very cool. Includes integrated link status/utilisation information.

  * View and control of device management status - traditional, hybrid, pure OpenFlow, all in one place. Change ports from one mode to another, within IMC.

  * Classic network monitoring information and statistics, gathered through SNMP, NetFlow, NQA are all still there - but now with added OpenFlow-specific information.

  * Application "Library" managed from IMC, which can be used to deploy applications to the SDN controller, and then control them. Management functions will include license management.

  * Networks can have multiple controllers, and IMC can show forwarding rules implemented on a device per-controller. (I don't really understand how a switch can have multiple controllers pushing rules to it at once, but they tell me this is possible).

  * Backup and Restore Controller Configuration.

  *  Troubleshooting options for tracing a flow across the network. If this works well, it will become a crucial part of the modern network engineer's toolkit.

  * IMC Configuration Management tools could use CLI, etc. to provision and configure new device, enable OpenFlow, and make it available to the controller.



Note this list is based upon my notes from the session, not official HP statements. It may be inaccurate, and besides, what they end up shipping may not exactly match what they'd like to ship.


## What about Licensing?


I don't know exactly what the licensing models are going to look like. My guess is that the IMC SDN module will be just another licensed module, at a reasonable price. But what I'm not too happy about was hearing that IMC will have functions for managing SDN application licensing. I understand that companies are looking to get a return on investment for any SDN applications they write, but I worry that great products will get killed with onerous licensing schemes.

It would be a terrible shame if applications were licensed in a way that could get out of hand as networks grow, or just have such a convoluted scheme that no-one can understand it, and they give up. Hopefully we see some sane, understandable licensing models, that allow companies to control cost, and get clear value.


## And other Controllers?


HP stated that their intention is to have an "Open" platform, that can integrate with other SDN controllers, provided they have some form of standardised API. There's a lot of flux in the industry at the moment, so initially HP will be developing for their own controller. Beyond that they will be watching the market to see which controllers gain dominance. If say [Open Daylight](http://www.opendaylight.org) gains significant market traction, then they will offer integration with that. I suppose in theory it could become like SNMP - offer a standardised interface, but let Enterprises extend it in a way that others can integrate with.

There was one comment that made me a bit nervous during this discussion: Someone was talking about how the HP OpenFlow controller will be fully compatible with OpenFlow 1.3 devices...but that HP has _some of their own extensions to OpenFlow 1.3_ (emphasis mine) that they use between HPN gear and their controller. This sort of thing disturbs me. I don't know the full details of what those extensions are. I understand that OpenFlow is a developing standard, and it may not yet fully define everything required for future applications. I just hope that this is primarily a timing issue, and that we will see those HP extensions become part of the next version of OpenFlow.


## When can I get my hands on it?


HP is planning on releasing this later this year. The code is in alpha right now, and I've seen some tightly controlled demonstrations, so it's not completely vapourware. As I understand it, it is being beta-tested in live customer networks too. Unfortunately not in any networks I have access too!

I'm pleased to see that HP is trying to create a vision that encompasses SDN within a broader Enterprise network environment. I see SDN initially being used in pockets of the network, so having a pure-SDN management system makes little sense. The value comes from integrating it into your overall service management systems. I'm very keen to see this product ship - I hope they can execute on the vision.

{% include note.html content="N.B. HP covered my costs to attend HP Discover. There was no direct monetary compensation. They did not seek to unduly influence what I write." %}
