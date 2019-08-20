---
author: lindsay
date: 2013-12-03 20:00:42+00:00
layout: post
slug: ccie-preparation-lab-equipment
title: 'CCIE Preparation: Lab Equipment'
categories:
- CCIE
series: 'CCIE Preparation'
tags:
- CCIE
- study
---

{% include series.html %}

CCIE study requires a large amount of hands-on practice, to gain the configuration speed needed for the lab exam. So the question is: Where and how do I gain that hands-on time? That then leads into: What are these topologies people talk about? GNS3 vs rack rentals vs buying my own equipment - which one should I do? And what are rack rentals anyway?

These are all common questions for people starting out on the CCIE path. Let’s go through the options, and see why I usually recommend rack rentals.

## Topologies

First a word on topologies. You will hear people talk about the “INE topology,” the “IPExpert topology,” “Narbik’s topology,” etc. The different training vendors have each developed their own base physical topology for a group of routers and switches. All of their workbooks and exercises will assume you have that topology. Usually there will be around 6 routers and 4 switches, plus 3 Backbone routers. Backbone routers act like third-party systems: You can peer with them and exchange routes, but you can’t login to those systems. The real lab uses 1841 and 3825 routers, and 3560 switches, but specific models will vary amongst vendors, as they try to keep costs down.

There will be many cables interconnecting the switches, and the routers will all have multiple Ethernet and Serial interconnections. The beauty of this setup is that all kinds of logical topologies can be created without needing to re-cable the rack. By choosing which ports you enable, and which VLANs you use, you can create a variety of logical topologies. You never touch the cables once they’re in place. When working on a focused lab, you may only use a few devices, ignoring the rest. If you’re working on a full-scale lab, there will usually be an initial configuration loaded that establishes the logical topology. Note that backbone router configurations NEVER change - this will mean that they are often advertising many different routes via different protocols, but you will only use some of them in each lab.

This is identical to the way the real CCIE lab operates - there is a fixed physical topology, with many logical topologies. Many candidates struggle with this at first, as they are used to networks where the logical and physical layouts are more closely aligned. They also don’t quite get it that you can run a lot of cables, but if you don’t enable those ports, then you can just pretend they’re not there.

Here’s an example of the INE physical topology:

[![INE LAN Topology](/assets/2013/10/5ccb9b7b08f659671616b57db65b491100405fd6.jpg)](http://labs.ine.com/workbook/view/rs-rack-rental/task/physical-lan-cabling-Njkw)

{:.image-caption}
INE LAN Topology

[![INE WAN Topology](/assets/2013/10/INE-WAN-Topology.jpg)](http://labs.ine.com/workbook/view/rs-rack-rental/task/physical-wan-cabling-Njkx)

{:.image-caption}
INE WAN Topology

If you have enough routers, you can actually re-cable your rack to switch between the different vendors. Because they all use slightly different models and linecards, the interface names will change slightly. No big deal though, a simple bit of sed and awk will update your initial configurations. Or Find/Replace if you’re lacking in Unix skills.

Note that all racks will also have a console server, for console access to all devices. This is the only practical way to access devices where you are configuring all network-related settings.

## Real Equipment

Every aspiring CCIE would like to have their own equipment. A rack full of kit, that they can use whenever they like, full access, no waiting, no latency, complete control. Sounds very cool, but it comes at a cost of both time and money.

Most students have a limited budget, so they start looking for second-hand equipment, and try to figure out what older equipment can be substituted - e.g. Using some 2621XMs instead of 1841s, or swapping two of the 3560s for 3550s. There are trade-offs in doing this - the key is in trying to figure out how much it matters. If you try to use really old equipment, it can’t run the right IOS (12.4(15)T), or may not have the right feature set - e.g. all switches must be L3-capable. If it’s only a few small differences in features, don’t worry too much - just use rack rentals for those specific features, such as QoS on 3560s.

There are several problems with running your own rack of gear:

* It can be time-consuming and expensive tracking down and assembling all required equipment. If you live in somewhere like New Zealand, there’s not a lot of suitable second-hand equipment available.
* You have to pay for the power to run this gear, and maybe extra cooling. You will probably want to get a web-accessible power switch so you can shut it down when not in use. Don’t forget about noise and space too - your partner will probably not want a half-rack of gear in the bedroom.
* Lab version changes may make your gear obsolete, particularly if you’re already running older equipment. This will significantly drop the resale value. Don’t pay much for anything that can’t run IOS 15.x - the next lab version will need 15.x.
* Loading initial configurations, saving and restoring sessions and rebooting devices can be time-consuming. You should develop/steal some scripts to manage this, but this may be challenging, depending on your background.

Because of these issues, I generally don’t recommend buying your own gear. The only circumstances where it makes sense is if your employer has a lot of spare equipment, and rack space to run it, or if you have very poor/non-existent Internet connectivity.

## GNS3

[GNS3](http://www.gns3.net/) is software for Windows, Mac and Linux that offers hardware emulation of Cisco devices, combined with a graphical interface for interconnecting devices. You pick the device to emulate, load up a real copy of IOS, and away you go. Sounds awesome right?

It is - up to a point. There are a number of challenges:

* Not all devices can be emulated, especially newer hardware. This will cause problems in future when 15.x IOS versions are required.
* You need to get your own copy of IOS from somewhere.
* Emulation is hardware-intensive. You will need a reasonably powerful system to emulate a large topology. Many people use a dedicated high-end desktop system for this, and connect to it from their laptop.
* Switching requires specific hardware, and can’t be done directly in GNS3. You can do basic switching tasks, but not QoS.
* Some features like Multicast and NTP are unreliable, leading to much hair-pulling as you try to debug your config, only to find your config is fine.

You can mix GNS3 and real switches, using a “breakout” switch. Under this model, you could use 4 x 3560 switches and 1 x 3750, and you can map the virtual routers to the real switches. This lets you create more realistic topologies, without having to invest in real routers.

It is easy to spend a lot of time configuring your hardware and tuning GNS3 to work just right. Because of this, I don’t recommend GNS3 for anything complex. If you are determined to follow this approach, make sure you look at some of the great tutorials out there on setting this up - there’s plenty of other people who have already figured out what does and doesn’t work. Don’t forget that you don’t have to have everything working perfectly - you can always use rack rentals for those features you can’t test on GNS3.

## IOU/VIRL/CML

Cisco has been using IOU (IOS On Unix) for the CCIE Lab Troubleshooting section for several years. This is IOS compiled to run under Linux or Solaris. This is NOT the same as GNS3, which is emulating hardware. Because it’s not emulated, it requires far fewer resources, and you can easily run 10 instances in under 1GB of RAM, with no significant CPU impact. These instances can be connected to each other, to form complex topologies.

Cisco offers [access to IOU running on their systems](https://learningnetworkstore.cisco.com/market/prod/listSubCatLearnLab.se.work?TRGT=85&/nxt/rcrs/=2559), at a price. There are also unsanctioned copies of IOU available if you know where to look. There are Layer-2 and Layer-3 images available, although the Layer-2 images do not work well.

In future this will change, with Cisco working on CML, aka [VIRL](http://virl.cisco.com/), which will offer a fully sanctioned method of running IOU. Depending on the exact feature set that Cisco releases, VIRL may replace GNS3, and could come to dominate the CCIE study market.

Currently, IOU is a good choice for practising all non-switching related tasks, as it can be quickly spun up in a Linux VM. It cannot do everything, and any time a bug is suspected, you should switch to real hardware, rather than wasting time. But it’s perfect for a quick BGP + EIGRP over Frame Relay test.

## Rack Rentals

Many companies offer online access to a rack of equipment. You buy access in blocks of time. Some vendors sell access by the hour (INE), others in 4 hour blocks (IPexpert), some by the month (Narbik). During your scheduled time, you have complete control over a pod of equipment. You will get console access to all systems, and generally you’ll be able to control the power too. Good vendors will also include configuration management tools, to load a lab’s initial configuration, to automatically save all your configs, and to restore from previous sessions.

You don’t need to worry about power, hardware failures, Cisco support contracts, finding the right IOS, or anything - it’s all taken care of for you. You just buy access as you need it. Cloud computing for CCIE candidates.

Where the rack rentals are associated with a training vendor, the topology will match that in their workbooks. Where the rack rentals are not associated with a specific training vendor, they will tell you what sort of topologies they can offer - some may have more than one option.

Pricing has been steadily falling for rack rentals. Current pricing:

* [INE](http://www.ine.com/rack-rental-tokens.htm) - $4USD per hour, or as low as $3.18USD per hour if buying large blocks.  Racks available in 30-minute increments.
* [IPExpert](http://www.ipexpert.com/Cisco/CCIE/Routing-and-Switching/Rack-Rental) - $15USD for a 4-hour block, dropping to $10USD for 4-hour blocks if buying in bulk.
* [Narbik](http://www.micronicstraining.com/) - $400USD/month, dropping to $300USD/month for bootcamp & workbook customers purchasing 3 months or more. Note that here you get dedicated access for the entire month, 24/7.

If you use rack rentals, then you will not waste any time setting up or configuring equipment, or debugging GNS3/IOU issues. You will be able to just dive straight into learning whatever core technology it is that you’re studying that day. No wasted effort == maximum study value.

Previous limitations to rack rentals were the sizes of the blocks you had to buy, and availability. INE used to have 6-hour blocks, with variable pricing based upon demand. These blocks didn't always work for your timezone. There were also issues with extremely limited rack availability when large bootcamps were running. Could cause unnecessary stress if your lab date was rapidly approaching. These issues are now mostly resolved, with more racks, and smaller blocks of time.

Because rack rentals let you maximise the utility of your study time, I now recommend that candidates use this as their primary means of practising hands-on configuration. It is a very cost-effective solution, with the least distraction. A few clicks to load up your desired config, and start labbing!

## Conclusion

Remember: Your goal is to become a CCIE. Remove all distractions, and focus on your core goal. Your study time is limited - make the most of it. Time spent learning the intricacies of GNS3 is time that would be better spent learning the intricacies of OSPF. Time spent pasting configurations into 10 devices is time better spent writing configurations. So use GNS3/IOU if you need to, but only for simple setups, and technology-focused labs. Use rack rentals for all full-scale labs and hardware-specific features. If you ever run into anything that might be a GNS3/IOU issue, spend no more than 10 minutes on it before moving to real equipment.
