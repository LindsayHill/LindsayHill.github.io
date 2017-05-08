---
author: lindsay
date: 2013-06-07 18:00:16+00:00
layout: post
slug: nnmi-free-edition
title: NNMi Free Edition
categories:
- HP NNMi
tags:
- hp
- nnmi
---

HP has recently released a [free version of HP NNMi](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1170657&jumpid=ex_r11374_us/en/large/eb/go_nnm#tab=TAB2). This gives you a perpetual license to run NNMi, with some limitations. It’s not completely crippled freeware, and may be worth a look if you want basic fault monitoring in a lab or small network.

NNMi (Network Node Manager i) is HP’s flagship network monitoring and management product, for very large networks, with very deep pockets. It’s a descendant of the legacy HP OpenView suite, something many engineers have less than fond memories of. The product was completely rewritten a few years ago, and looks nothing like the old product. NNMi is the base network discovery and fault polling system. HP sells add-on “Smart Plug-Ins” or SPIs, to add features such as performance data collection & monitoring, NetFlow analysis, IP SLA monitoring, etc. There is also a partially integrated Configuration Management product called “Network Automation.” Theoretically, NNMi is meant to tie into the rest of HP Software’s BSM product line, which pulls together all the disparate monitoring tools across servers, network and applications. None of this product suite is to be confused with HP IMC, which is another network management product sold by HP, with a completely different ancestry (It came via the 3Com acquisition).

**Inclusions:** The free version gives you full network discovery, and most fault monitoring capabilities of NNMi. Give it some SNMP strings, and network ranges, and NNMi will auto-discover your network, learn the interconnections, and give you fault monitoring for all nodes, hardware components, and interfaces (Up/Down status). You can layout your devices on a map, and configure basic alerting. Note that you will need to spend the time to learn how to configure node groups and monitoring policies. Monitoring interfaces that are not connected to other SNMP-capable devices can be a bit challenging to figure out if you’re not used to NNMi.

**Limitations:** This version is limited to 25 nodes, and does not include any “Advanced” features, or support for Smart Plug-Ins. This means no VRRP/HSRP monitoring, no LAG monitoring, and no integration of other products. None of those are overly significant for only 25 nodes, but crucially, no SPIs means no performance monitoring. No threshold alerting for any monitored metrics, and no fault or performance monitoring of custom OIDs. There’s also a limit of 3 concurrent users - probably not a big deal if you’ve only got 25 devices under management! Note that it probably doesn’t include any IPv6 capabilities either - that is an “Advanced” feature with the normal product.

**Should I try it?** NNMi installs with a 60-day trial license, that allows full access to all features, with a limit of 250 nodes. If you’re evaluating NNMi, I strongly recommend that you install this trial license, and take the time to get to know the product, to make an informed decision as to if it’s the right fit for your network. With the advent of the free version, I continue to recommend using this trial license. But if you need to have a semi-permanent lab, and you only need the basic features, this could work well. I wouldn’t recommend using this as a learning tool - if you’re looking to learn about Network Management, SNMP, NetFlow, etc., then you really need to have all features available to you. Configuring custom OID monitoring is an important part of learning about SNMP, and you won’t be able to do that with the free version. Either use the demo license, or look for other tools.

I’m happy that HP is making moves like this to make their HP Software products more accessible, but I’m not convinced that the move makes sense. There’s a big jump in price between this version and the paid product, especially when you add on the SPIs that make it useful. How many people are going to use this free product for an extended period, then decide that they want to make that jump, and can get the budget to do it?
