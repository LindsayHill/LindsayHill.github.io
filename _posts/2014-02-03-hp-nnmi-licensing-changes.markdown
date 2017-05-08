---
author: lindsay
date: 2014-02-03 02:15:35+00:00
layout: post
slug: hp-nnmi-licensing-changes
title: HP NNMi Licensing Changes
categories:
- HP NNMi
tags:
- hp
- nnmi
---

HP has made some changes to the way Network Node Manager (NNMi) is licensed. These changes are a Good Thing, as they address two key pain points - license calculation, and access to extra features.

Previously NNMi had a base per-node license for fault monitoring. You could then buy add-on "Smart Plugins", or iSPIs, to give you extra features, such as performance monitoring, NetFlow, IP SLA, MPLS, Telephony, etc. The iSPIs each had their own license calculation method, involving "points", which could be shared between some add-ons (but not all). Trying to figure out exactly how many licenses you needed to buy could be a complete nightmare, and partners/HP often struggled to figure it out.

As a result, customers might end up either over-licensed (with associated higher support costs), or they didn't have access to some of the advanced features that would greatly improve their overall network monitoring. Going back to the cheque-book holders for more money for licenses wasn't politically acceptable, so they made do without.

The new model vastly simplifies things - there's two tiers: a base tier that includes some commonly-used add-ons, while the top tier includes all available add-ons. Licensing is simply per-device, making it far simpler to calculate. Bundled features rather than _à la carte_ pricing leads to greater adoption of enhanced features, leading to improved customer satisfaction and maybe retention.

If you're using NNMi, I recommend that you talk to your partner/HP account manager, and find out how you can migrate to the new model. There are of course a few conditions, and no doubt there are a few cases where it doesn't work out right, but it should benefit the vast majority of customers. Hopefully you'll find that you can get access to more features at no extra cost.
