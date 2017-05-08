---
author: lindsay
date: 2014-11-13 18:00:23+00:00
layout: post
slug: cumulus-in-the-campus
title: Cumulus in the Campus?
categories:
- Routing &amp; Switching
tags:
- Cumulus
- SDN
---

Recently I've been idly speculating about how campus networking could be shaken up, with different cost and management models. A few recent podcasts have inspired some thoughts on how [Cumulus Networks](http://www.cumulusnetworks.com/) might fit into this.

In response to a PacketPushers podcast on [HP Network Management](http://packetpushers.net/show-209-hp-networks-network-management-sponsored/), featuring yours truly, Kanat [asks](http://packetpushers.net/show-209-hp-networks-network-management-sponsored/#comment-1649657424):

> For me the benchmark of network management so far is [Meraki](http://meraki.cisco.com/) Dashboard - stupid simple and feature rich...
> Yes - it’s a niche product that only focuses on Campus scenarios, Yes - it only supports proprietary HW. But it offers pretty much everything network operator needs - detailed visibility, traffic policy engine with L7 capability, MDM and you can hit it and go full speed right away.

How long will it take HP to achieve that level of simplicity/usability?</blockquote>
 
 

He’s right about the Meraki dashboard. It’s fantastic. Fast to get set up, easy to use, it’s what others should aspire to. But there’s a catch: It only works with Meraki hardware. Keep paying your monthly bills, and all is well. But what if you’ve got non-Meraki hardware? Or what if you decide you don’t want to pay Meraki any more? What if Meraki goes out of business (unlikely, but still something to consider). Will you be left with a pile of un-manageable hardware?


 
 ## Build on top of Cumulus?
 
 

If you wanted to create a platform with similar power, but the ability to use open hardware, how would you do it? The key is the software on the switches. You need to control this tightly. The best way is either to write your own complete Network Operating System (NOS), or write plugins that work with other NOSes.

This is where [Cumulus](http://www.cumulusnetworks.com/) could enter the picture. On “[Software Gone Wild](http://blog.ipspace.net/2014/10/cumulus-linux-in-real-life-on-software.html)”, [Matt Stone](https://twitter.com/BigMStone) talks about how Cumulus is maybe not ready for direct use by Enterprise, but that there could be a market for someone to create a management system that uses Cumulus switches.

Cumulus provides an open platform that could easily be extended. You can write plugins that bridge between your management platform and the underlying OS. It would be far easier than trying to write your own NOS from the ground up. Using Cumulus would give you far deeper access to the underlying switch workings than you'll get from Cisco or HP.


 
 ## Customer Options
 
 

It gives the customers flexibility too. If they decide they’ve had enough, they’re not stuck with un-manageable hardware. They could keep using Cumulus, or they could change the NOS to something like [Pica8](http://www.pica8.com), or [Big Cloud Fabric](http://www.bigswitch.com/sdn-products/big-cloud-fabric).

Or just maybe, they could use JunOS, EOS or even IOS, as [@mrtugs](https://twitter.com/mrtugs) was [speculating about](http://lamejournal.com/2014/11/11/response-cisco-arista-disaggregating/)? This is of course the broader dream of network disaggregation.


 
 ## Do the costs add up?
 
 

Maybe not. Cumulus in the campus doesn't currently stack up. That's not surprising - their initial target market has been the data centre. The cheapest switches on the Cumulus HCL are the likes of the [Quanta T1048-LB9](http://whiteboxswitch.com/collections/featured-poc-bundles/products/quanta-t1048-lb9), which is $2,449.00. A Cumulus 1G license adds another $499/year.

That price isn’t hugely different from what I might pay for a reasonable Cisco or HP campus switch. So I’d be taking a risk on a relatively new platform, without saving much up front. The key would be in the additional capabilities the platform offers me.

What if Cumulus was supported on a cheaper platform? Something equivalent to a [Cisco Small Business Switch](http://www.cisco.com/c/en/us/products/switches/small-business-300-series-managed-switches/index.html), or a [D-Link DGS–1510–52](http://us.dlink.com/products/business-solutions/52-port-gigabit-smartpro-switch/).

What would that do to the campus market? My problem with the low-end switches has primarily been the poor software (see what John found with [his cheap switch](http://lamejournal.com/2014/11/06/quanta-lb4m-cheap-white-box-switching/)). But if we had more powerful software available for cheap switches, what would that open up? Even without someone developing a Meraki-like management platform, it would massively shake things up.
