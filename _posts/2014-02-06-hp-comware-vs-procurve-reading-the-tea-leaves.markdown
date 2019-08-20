---
author: lindsay
date: 2014-02-06 19:00:27+00:00
layout: post
slug: hp-comware-vs-procurve-reading-the-tea-leaves
title: 'HP Comware vs ProCurve: Reading the Tea-Leaves'
categories:
- Routing &amp; Switching
tags:
- Comware
- hp
- procurve
- wireless
---

HP acquired 3Com in 2009. This added a large range of routing, switching and wireless networking to HP - but they already had existing ProCurve wired and wireless hardware. Clearly there's overlap, so what's the future of HP Networking? Where do they go with hardware and software? Here's some speculation on my part.

{% include warning.html content="Note: I have no inside information on this. I am simply speculating, based upon available public information. Do your own thinking before making major purchasing decisions." %}

## History: Two Networking Lines

HP's ProCurve group has its roots going back [as far as 1979](http://en.wikipedia.org/wiki/ProCurve). Through the 2000s, HP developed and sold their own software, hardware and custom ASICs, powering their ProCurve line of switches. These switches are aimed at the middle of the market - those organisations that wanted reasonably powerful, fully managed access switches, but didn't need/want the complete featureset and high price that Cisco was offering. They offered fixed-port, stackable and modular switches, but they stuck to a focus of switching, with some basic routing capabilities.

In 2008, they had approximately 8% of the [Ethernet switch market](http://h30507.www3.hp.com/t5/HP-Networking/Ethernet-Switching-Market-Share-Did-HP-eat-up-that-much-share/ba-p/120753#.UvNIH0KSyWE). Obviously Cisco was the clear market leader, but the other players had < 5% market share.

3Com also goes back to 1979, developing some of the earliest Ethernet products. They seemed to lose their way in the late 90s / early 2000s, pulling out of the high-end market. During the 2000s, they had a joint venture with Huawei, known as H3C. The products of this joint venture were primarily sold within China. The 3Com product range was much wider than HP's, spanning high-end Service Provider Routing & Switching through to desktop access switches.

In 2009, partly as a result of Cisco's move into servers with UCS, HP acquired 3Com, significantly boosting their network offerings. The problem is that the network line now has some overlaps, and there has not yet been a significant shakedown.

## Codebase: One Source Tree, Multiple Images

I asked about the future of ProCurve/Comware at HP Discover Barcelona 2013, and how they might be combined. I was told that the plan is to have "One source tree, with multiple images." There would be separate drivers for different models/ASICs, and individual binary builds for each device type. It's not quite as elegant as Arista's approach of running one single binary image across all devices, but HP has to support a much wider range of hardware.

From a development perspective, this makes sense. One source tree makes life much simpler - why write two versions of BGP daemons? Comware has a far more feature-rich history, as it has been used in high-end routing platforms, while ProCurve was focused on the access layer. So we'd have to assume that Comware will be the basis of underlying code. But what about the interface? Are all ProCurve admins going to have to get used to a Comware CLI? Or the other way around?

There's no reason why we couldn't have different CLI "modes", the way BNT can switch to "Industry-standard CLI". The CLI & configuration is quite separate to the underlying code - so it should be easy to have a translation layer that could convert from either Comware or ProCurve-style configuration. After all, the typical network admin doesn't have any visibility of the underlying OSPF processes - they're just used to a certain command structure.

But I don't think we'll switch "mode" - I think we'll see a continuation of the current trend of adding Comware-style commands to ProCurve devices. Each new ProCurve software update seems to add more Comware-style CLI options, whilst retaining the ProCurve options. If I use Comware "show" commands, the output is formatted Comware-style. But if I enter Comware-style configuration, it is converted on the fly to ProCurve syntax. I don't see a lot of features or configuration style from ProCurve coming into Comware though. So I suspect HP's goal to get ProCurve admins gradually used to the Comware style, so that at some point it will switch over: Comware will become the base, with a few ProCurve style commands to help transitioning network admins.

Of course, looking further ahead, it shouldn't matter - because we'll push out changes from the controller, and the underlying operations at a switch level will be abstracted away, right? Yeah, well, that might take a bit longer to roll out everywhere.

## Hardware: Both Lines Continue, with Consolidation

Historically ProCurve systems have used custom ASICs (the ProVision ASIC), while [Comware systems have used merchant silicon](http://www.networkcomputing.com/data-networking-management/hp-on-right-track-with-two-network-oses/231002425). HP has been working with OpenFlow since the early days, initially with their ProCurve line. They also feel that the ProVision ASIC helps them meet a certain price/feature point that they couldn't do with merchant silicon. They've certainly achieved reasonable success doing this at the desktop access layer.

There is no reason why HP can't continue to pursue both a custom ASIC and a merchant silicon strategy. For specific use-cases, custom silicon is better. For others, merchant silicon can better deliver the desired price/performance/feature point. See Cisco's recent "[Merchant-Plus](http://ethancbanks.com/2014/01/07/comparing-aci-nsx-on-network-world/)" strategy for an example of this. It's just a question of selecting the right product for the right use-case.

Looking at recent developments, there's been some noise around [new product launches with Comware history](http://www.networkworld.com/community/blog/hp-networking-finally-rolls-out-data-center-switch) - e.g. the 5900, 11900 and 12900 released in 2013. But the [ProCurve 2920](http://www.theregister.co.uk/2013/02/19/hp_hybrid_switches_sdn_networking/) was also released last year - so clearly investment is continuing in both lines.

The [full switch product list](http://h17007.www1.hp.com/us/en/networking/products/switches/index.aspx#tab=TAB2) has 46 different models listed. Clearly there are some that could be removed, where an update has been released for the same line - e.g. I don't know why they bother listing both the 2910 and the 2920, or the 5900, 5920 and 5930 - just get rid of the old ones already.

But try using the [switch selector](http://h17007.www1.hp.com/us/en/networking/products/switches/selector/index.aspx#) - let's say I want a fixed-port, 1Gb PoE+ switch, with 10G uplinks, and stacking capabilities. That returns the 5500 and the 3800 - one runs Comware, the other runs ProCurve. How am I supposed to know which one I should use? I'm probably going to go with whatever I'm used to - ProCurve or Comware.

My guess is that HP wants to consolidate some of these lines, but they can't do it until they've got the software sorted out. Once they can run the same OS on either hardware, then there are far fewer differentiators, and we'll start to see some lines get retired. This will probably appear as natural attrition - newer lines like the 2920 and 5930 will stick around, with further software updates, while some of the older lines will start getting retired, and won't see replacement.

## So What Do I Buy?

Whatever fits your needs best, with a caveat: if I had no other HP equipment, I'd probably prefer to get Comware equipment, as that seems to be the clearer software direction. Whatever branch you prefer, make sure you're buying the newest variants - e.g. the 2920, rather than 2910.

If you are running ProCurve gear, I would recommend moving to recent software releases, so you can start getting familiar with the Comware CLI.

In an upcoming post, I'll take a stab at what I think is happening with the wireless gear - there's overlap there, and I think the future is clearer, and we'll see the lines consolidated soon.
