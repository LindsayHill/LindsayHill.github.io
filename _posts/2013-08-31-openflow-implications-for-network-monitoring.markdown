---
author: lindsay
date: 2013-08-31 08:23:39+00:00
layout: post
slug: openflow-implications-for-network-monitoring
title: OpenFlow implications for network monitoring
categories:
- NMS
tags:
- OpenFlow
- SDN
---

I've been reading more about OpenFlow recently, and something that was pointed out to me was that OpenFlow offers features that could give us deeper insights into our traffic flows, without necessarily modifying them. Whilst not being a full replacement for NetFlow/sFlow, it would be far better than just guessing at what an application is doing. I'm pretty excited about the possibilities.

Here's some relevant parts of the [OpenFlow v1.3.2 specification](https://www.opennetworking.org/images/stories/downloads/sdn-resources/onf-specifications/openflow/openflow-spec-v1.3.2.pdf):




	
  * _4.5 Reserved Ports:_ "Optional: **NORMAL**: Represents the traditional non-OpenFlow pipeline of the switch" - this means the packet is sent to "normal" L2/L3 processing

	
  * _5.8 Counters:_ "Counters are maintained for each flow table, flow entry, port, queue..." Counters Per Flow Entry include Received Packets, Received Bytes, Duration.



If we put those together, it means that we could use OpenFlow to push out a rule to our switches that identified traffic we were interested in - e.g. matching on src/dst IP - without actually change any forwarding behaviour. By identifying traffic in a specific rule, we can collect statistics on specific traffic volumes. So now we've got a distributed statistics collection system. The analysis is already done at the switch level - the reporting should be relatively straightforward, with simple processing on your management system.

This isn't the same as traditional NetFlow/sFlow, where I export all flows, and have heavy-lifting systems to analyse that data and report on traffic. We have to know what traffic we want to report on, and push a rule to collect statistics. It won't tell me what happened last week, if I wasn't collecting statistics at that time. But on the flipside, I can push this out to any switch in the network - it's not like NetFlow or sFlow where the traffic must be passing through one of the points in the network where I'm collecting Flow data. Currently NetFlow/sFlow processing requires reasonably heavy CPU/disk to parse and store the data. But if we're pulling simple statistics, in a similar way to collecting SNMP data, that's orders of magnitude easier to collect and store.

To make informed decisions about controlling network flows, it helps to have good information about what's actually happening on your network. OpenFlow statistics aren't a panacea, but it might just help us get that visibility, in a simple, scalable way. In that case, I'm all for it. What else might OpenFlow bring us?
