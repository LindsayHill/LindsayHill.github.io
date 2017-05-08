---
author: lindsay
date: 2013-12-17 02:23:54+00:00
layout: post
slug: hp-rams-ending
title: HP-RAMS Relationship Ending
categories:
- HP NNMi
tags:
- hp
- RAMS
---

HP is discontinuing support for HP Route Analytics Management Software, aka RAMS. RAMS is an extremely powerful system for managing large networks, but it always struggled to fit in with the rest of the HP Software suite.

Traditional network monitoring systems poll a network via SNMP, and maybe collect data via NetFlow. They then try to build up a picture of the network through the data polled via SNMP - CDP neighbours, ARP caches, routing tables, etc. RAMS takes a different approach, and inserts itself into the network. It acts as a BGP peer, and OSPF/EIGRP/IS-IS neighbour. It then has full visibility of the network topology, and sees all routing topology changes. This lets you build up a much more accurate picture of the network topology, and lets you replay routing changes to see how the network behaved during an outage. You can then take that a step further, and perform network modelling, and "What if?" scenarios. It can identify single points of failure, and plot paths between sources and destinations. Awesome power.

RAMS was licensed to HP Software from [Packet Design](http://www.packetdesign.com/). HP integrated it with Network Node Manager, providing cross-launching, alert integration, etc. But no more - that relationship has ended, and customers who want the product can deal directly with Packet Design. Existing customers will be able to transition their support agreements across.

The official notification is [here](http://support.openview.hp.com/encore/rams.jsp), with a [Customer Letter](http://support.openview.hp.com/pdf/rams-customer-letter.pdf) and [FAQ](http://support.openview.hp.com/pdf/rams-customer-faq.pdf). Key dates:

> September 28, 2013: Product discontinuance announced
> November 25, 2013: End of sale
> October 31, 2016: End of support

I believe that the reason for this relationship ending is that RAMS really requires a Network Engineer to sell/install/configure it, as opposed to Management Software Consultants. Most engineers that approach it from the NMS side can handle the level of networking knowledge required for SNMP monitoring, but once you start talking about OSPF + BGP, it becomes a specialised skill, and management software people just don't get it.

HP's website still lists [RAMS as an add-on for NNMi](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1172914#tab=TAB1), but don't you believe it. If you need that functionality, go direct to [Packet Design](http://www.packetdesign.com/).
