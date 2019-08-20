---
author: lindsay
date: 2013-10-06 17:00:29+00:00
layout: post
slug: cisco-prime-multivendor
title: 'Cisco Prime Lack of Multivendor Support: Who Loses?'
categories:
- NMS
tags:
- cisco
- NMS
---

Recently I've been thinking about [Cisco Prime Infrastructure](http://www.cisco.com/en/US/products/ps12239/index.html), and Cisco's continued resistance to supporting non-Cisco equipment. I've been wondering if this is good for Cisco, and if they should face reality, and add support for non-Cisco devices.

Cisco Prime Infrastructure is an NMS/NCCM product from Cisco, aimed at Enterprises running all-Cisco network equipment. It provides fault and performance monitoring, configuration management, network visibility, and provisioning, across Cisco wired and wireless products. [According to Cisco](http://www.cisco.com/en/US/products/ps12239/index.html):

> Cisco Prime Infrastructure empowers IT departments to more effectively manage their networks and the services they deliver. This scalable, integrated solution tightly couples end-user awareness and application performance visibility with comprehensive lifecycle management of wired and wireless access, campus and branch networks.

Fairly sickening marketing-speak, but you get the idea. Cisco Prime Infrastructure is the direct descendant of Cisco Prime LMS, previously known as CiscoWorks LMS. CiscoWorks has a bad reputation amongst grizzled network engineers, but you can't deny the number of customers: 30,000 (in 2011, the most recent I could find). No doubt many of those customers got the product for free with a large hardware deal, and will never install it. But we'll gloss over that for now. It also gets reasonable [reviews](http://showbrain.blogspot.co.nz/2012/06/not-your-fathers-ciscoworks.html) [these days.](http://networkingnerd.net/2012/04/13/cisco-borderless-network-field-day-3/) Because we all love Gartner around here, this is what they had to say in their [2011 NCCM MarketScope](http://blogs.gartner.com/jonah-kowall/2011/11/01/network-configuration-and-change-management-nccm-marketscope-published/):

> "Cisco received high marks for overall viability and sales execution...Cisco's NCCM strategy and vision is well-thought-out and correct for a network equipment manufacturer; however, its product and strategy ratings were somewhat constrained, **because only Cisco devices are supported.**(emphasis added)

So it all sounds like it's a product well worth considering...but with a catch: Only supports Cisco products. Check the [Supported Devices list](http://www.cisco.com/en/US/products/ps12239/products_device_support_tables_list.html). Any device, with any OS is supported for Performance Monitoring, Device Availability, and LLDP Neighbour Discovery. But what about all the other cool features around configuration management? You're out of luck. No support for any non-Cisco equipment, and **no way to add any**.

If your network is small, and you've only got a few non-Cisco devices, then no big deal, right? But what if you've got say 200 Cisco devices, and 100 Juniper switches? It's too many Juniper devices to handle manually - so you have to find a non-Cisco NMS. JunOS Space probably won't help you either - as far as I can tell, it has the same limitations as Cisco Prime when it comes to gear from other vendors. You need to find a 3rd-party NMS/NCCM system that will work with a heterogeneous network.

Let's think about something for a minute: What do you think would happen if Cisco Prime Infrastructure supported non-Cisco equipment? Assuming the product does what it says on the tin, we could reasonably expect to see more Cisco Prime sales, particularly in networks that are 50-85% Cisco. People who had shunned it previously could now consider it. What about hardware sales? This is a more debatable question. Would we see reduced sales of Cisco gear, as previously 100% Cisco shops introduced other vendors. Or would we see a halo effect, where people use Cisco Prime, realise how much better it is with all Cisco gear, and go out and buy more 3850s?

> Well I guess that depends on whether Cisco is a hardware or software company, right? They keep telling us they want to be a software company. Do any of us believe that?

Note that I am not saying that Cisco should actively support non-Cisco devices - I just think they should have an infrastructure that allows customers to integrate other vendors. Provide a mechanism for customers to share those integrations too, and build up a community around the product. Who knows - maybe a few more companies will decide to get over their deap-seated loathing of Cisco Works, and give it another go? And if that works out well, maybe they'll feel a bit better about buying more Cisco kit.
