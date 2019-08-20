---
author: lindsay
date: 2014-02-07 19:00:18+00:00
layout: post
slug: hp-wireless-future
title: 'HP Wireless Future: Reading the Tea Leaves'
categories:
- Routing &amp; Switching
tags:
- Comware
- hp
- procurve
- wireless
---

Recently I [posted some speculation]({% post_url 2014-02-06-hp-comware-vs-procurve-reading-the-tea-leaves %}) about where HP is going with their Comware and ProCurve networking lines. But what about the wireless gear? Where's that going? As before, I have no inside information on this, and I'm just speculating based upon public information: Do your own research and thinking before making major purchasing decisions.

## Colubris or Comware?

HP [acquired Colubris](http://www.networkworld.com/news/2008/081108-hp-buys-colubris.html) in August 2008, greatly expanding its ProCurve division wireless networking capabilities. This gave HP their "MSM" Wireless controllers. But the 3Com acquisition introduced its own APs and Controllers - the WX series of controllers. Since then, HP has been selling both ranges of controllers and access points. The MSM controllers use their own OS, which is neither Comware nor ProCurve.

In 2013, HP launched the[ 830 Unified Wired-WLAN switch](http://h17007.www1.hp.com/us/en/networking/products/mobility/HP_830_Unified_Wired-WLAN_Switch_Series/index.aspx) with an integrated controller (similar to Cisco's 3850), and the [10500/7500 Unified Wired-WLAN Module](http://h17007.www1.hp.com/us/en/networking/products/wireless/HP_10500_7500_20G_Unified_Wired-WLAN_Module/index.aspx), for installing in high-end chassis switches. Both of these wireless controllers are using Comware OS.

Take a look at the [HP Wireless Selector](http://h17007.www1.hp.com/us/en/networking/products/wireless/selector/index.aspx). Play around with some different options for choosing different controllers - either MSM, WX or the newer Unified 830 or 10500/7500 module. Note that of the 8 Controller-Managed Access points, five of the newest APs (425, 430, 460, 466, 466-R) can be managed by any controller - either Colubris-heritage, or Comware-based.

So what do our data points tell us?

1. Engineering resources are going into Comware-based controllers (that's where we see new products coming from)
2. New APs can be switched between controller types, giving customers a clear migration path, no need to rip & replace
3. Therefore the long-term path is towards Comware-based controllers, using most of the already deployed APs.

But what about those existing MSM controllers people have deployed? Would they run Comware on that hardware? My guess is not. Given where HP is going with their [VSR 1000](http://h17007.www1.hp.com/us/en/networking/products/routers/HP_VSR1000_Virtual_Services_Router_Series/#.UvMePUKSym0) virtual router, my guess is that a purely virtual controller will be made available, and customers can migrate to that. What about those APs not supported by Comware controllers? They're only single-radio APs, so they'll probably age out of customer networks reasonably quickly. We would need to see a replacement for the [MSM317](http://h17007.www1.hp.com/us/en/products/wireless/HP_MSM317_Access_Device_Series/index.aspx) though, it is an elegant solution for hotel-room wireless.

## Conclusions

Firstly, don't panic too much. Make sure that any new APs you install are newer APs, and are supported by both sorts of controller. Replacing APs is much more expensive and time-consuming than replacing a controller. If HP does declare a "winner", then start planning a controller migration if required.

If you do have APs that are not supported by both controllers, then you should be starting to look at replacements. Given that the older models are all single-radio, you should have this as part of your planning anyway. And remember, there is no official announcement on this - I'm just engaging in a bit of speculation. You should definitely be talking to your HP account manager about the future of their wireless offering though - follow their advice on the right models to use.
