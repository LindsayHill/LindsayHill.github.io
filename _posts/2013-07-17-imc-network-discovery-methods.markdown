---
author: lindsay
comments: false
date: 2013-07-17 19:00:21+00:00
layout: post
slug: imc-network-discovery-methods
title: IMC - Network Discovery Methods
categories:
- NMS
tags:
- discovery
- imc
---

Auto-discovery can help you populate your NMS, and keep it up to date. HP IMC supports several methods of adding devices - either by manually adding them, or getting IMC to automatically discover them. Here's an overview of the options available to us:

* Manually add a single Device
* Batch Import
* From CSV
* From NNMi
* Auto Discovery - Basic
* Auto Discovery - Advanced
* Routing-Based
* ARP-Based
* IPsec VPN-Based
* Network Segment-Based
* PPP-Based

Let's take a quick look at what each of the options does, and when you would use each one. Note that for all discovery options there are common options - SNMP credentials, Login type (Telnet/SSH/None), and Login Credentials. You can use Templates to speed the entry of credentials. NB: Templates only take effect when initially applied to a device - if you update the template later, [it won't change that device]({% post_url 2013-07-21-imc-misconceptions-templates %}).

{% include warning.html content="Each discovery you run (scheduled or manual) can only try a single SNMP Community string. If you want to try multiple strings, you need to run multiple discoveries. Other tools can try multiple strings as part of a single discovery schedule. You can configure discovery to ignore SNMP failures, and add the device anyway, but you'll probably end up with a lot of irrelevant objects if you do that." %}

### Add a Single Device

To get here, go Resource -> Add Device. This takes one IP address, and SNMP/Login credentials. It adds a single device - this is best for when you've got a single device to add, and/or it has some unusual characteristics. Fill in an IP address, SNMP community strings, and Login Credentials. After filling in the details, IMC will go and try to discover the device. It will probe it using ICMP and SNMP, and decide if it can be added.

There is one little shortcut you can use if you've got a couple of similar devices to add - once you've added one, click "Clone to Add" - this will keep all the previous settings - you just need to set the new IP address. You might be tempted to use this option if you've got a few devices to add - but you're probably better off running a discovery against a range of IP addresses.

### Batch Import

If you have a large list of nodes to add, and you know the details for all those nodes, you can import a CSV file. This method is most appropriate if you maintain excellent documentation about all your devices, or perhaps you can easily extract a list from your current NMS. Go Resource -> Import/Export Device -> Import. Check the Online help for details on how to format the CSV file. As a quick alternative, you can export your current list to get an idea of the formatting. You'll need IP addresses, and SNMP credentials. Note you can't specify login credentials for this sort of import - you'll need to change it later individually, or across groups of devices using Resource -> Batch Operation.

[![Batch Import](/assets/2013/07/Batch-Import.png)](/assets/2013/07/Batch-Import.png)

If you have an NNMi server that has already discovered the network, theoretically you can import devices from NNMi. Go Resource -> Import/Export Device -> Import Devices from NNMi. This may work for some networks, but in my testing, it did not work for networks using DNS for hostname mapping. It's also of limited value if you want to configure integration between IMC and NNMi - for that to work, nodes must be added to IMC first, then discovered in NNMi through the integration.

### Automatic Discovery - Basic

The above methods are fine if you've got one node, or a well-defined list. But what if you're like most larger networks, and the documentation can't be fully relied upon, and the network is in a state of flux anyway? Most organisations don't have the operational discipline to ensure all new devices are added to documentation and monitoring systems, so we need to make sure we can discover all devices automatically - both for initial setup, and ongoing maintenance. IMC offers several methods of automatically discovering the network. All of these can be run as a once-off, or can be scheduled. You can configure multiple schedules, including different types of discovery.

A "Basic" discovery takes one or more ranges of IP addresses. It will go through each address in the range, trying to poll it, to see if it responds. Any addresses that respond successfully will be added to IMC. To get here, go Resource -> Auto Discovery, and then click "Go to Basic" if required. Note that you can only configure a login type "Telnet" for a Basic discovery - that's one of the reasons I recommend using Advanced discovery options.

### Automatic Discovery - Advanced

Going to Resource -> Automatic Discovery -> Go to Advanced will give you five options for running discoveries. These are much more flexible than the basic discovery methods.

**Routing-Based:** This method takes a list of seed devices. It uses SNMP to look at the routing table on each device, and tries to discover those gateway IP addresses. It will keep doing this, up to a maximum hop count (1-7, default 3). Good for dynamically routed non-Ethernet networks, particularly where you transit provider networks.

**ARP-Based:** Again, uses a list of seed devices, but this time looks at the ARP cache using SNMP, and tries to discover those IP addresses. Again, rinse and repeat, up to the maximum hop count. Works well for Ethernet networks. Note you might miss devices not currently in ARP caches, so this should be run multiple times.

**IPsec VPN-Based:** Looks at IPsec VPNs on a seed device, and tries to discover any encryption peer IP addresses. I think that the problem here is that you probably don't allow SNMP management via the peer IP - normally it's filtered to only allow SNMP to the loopback. You can specify a maximum hop count, just in case you've got a really complex VPN setup.

**Network Segment-Based:** Simple here - take a list of IP addresses, and work through each one, trying to find devices you can manage. Simple, but often good enough. You may well just want to start with this, particularly if you only have a few subnets, or well-defined management networks.

**PPP-Based:** This is interesting - find PPP connections on routers, and assume that the IP at the other end of the link is one you can try to discover. Problem is, not many people have a lot of PPP connections in their networks these days. Still, it works where ARP-based discovery can't. Needs one or more "Seed" devices. Can specify maximum hop count.

### Which Method(s) should I use?

It depends. For a simple network, if you're all connected via Ethernet, then ARP-Based will work OK. But you're probably more complex than that. If you've got a solid IP addressing layout, with a defined management network at each site, then just add that to Network Segment-based discovery, and run with that. If you don't have a clean IP address layout, you're probably going to have to run a mix of ARP-Based, Routing-Based, Network Segment-Based and ad-hoc manual discovery. Ugh.

What I'd really like to do is configure more complicated discovery methods - a mix of Routing-Based and ARP-Based should do the trick. Find neighbours through both routes and ARP cache, discover them, interrogate them for neighbours, rinse, and repeat. Limit by networks, not by hops. Actually, what I really want is the power of HP NNMi's spiral discovery in IMC. Come on HP, you're all one company, surely you can share some code?
