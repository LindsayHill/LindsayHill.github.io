---
author: lindsay
date: 2014-11-25 07:50:33+00:00
layout: post
slug: outsourcing-mistakes
title: Outsourcing Mistakes
categories:
- Opinion
tags:
- outsourcing
---

Outsourcing is complex, and there are lots of ways it can go wrong, or simply fail to deliver. I've put together a list of things that I see going wrong with outsourcing arrangements. Of course it's not exclusive!

There’s a few different types of outsourcing. It might mean procuring a commodity service - e.g. IaaS, or it might mean handing over your existing environment and staff to a third party. There's also a whole range of levels in between, but the usual deal is: Some part of your environment gets managed or delivered by someone else, according to the terms of a fixed agreement.

Here’s a few things I’ve learnt to watch out for (nb not all these items apply to all types of agreements):



## Not keeping up to date



If your outsourcer is managing your software, the contract usually covers applying security patches and bug fixes. But what gets missed is larger upgrades - e.g. ESXi 4.1 to 5.x. Everything goes OK for a while…and then your version goes End of Support.

It then becomes a major drama to get the upgrades sorted out. For financial purposes, you may not be able to do major upgrades as part of your regular outsourcing agreement (you might need/want to capitalise it). But your provider should be pushing to ensure these upgrades get done. Don’t get stuck with old versions.



## Being Overly Prescriptive



Be careful with how you structure agreements. Focus on desired service outcomes, not how to achieve those outcomes. For example, you could say “Must use Cisco Nexus 7009 switches” - or you could describe your bandwidth, latency and feature requirements. Or better yet, describe the application response times you need. Let the supplier choose the most appropriate way of meeting those requirements.

{% include note.html content="On a side note, I always wonder about Cloud Providers that make a big deal out of using Cisco + EMC hardware. Why should I care? It tells me that they're probably going to be expensive, and using [EMC is no guarantee of data safety](http://www.theregister.co.uk/2014/07/04/dimension_data_in_cloud_outage/)." %}




## Fixed Pricing that Doesn’t Reflect Changing Costs & Technologies



Let’s say you negotiate with your provider for 100MB mailboxes for staff. 3 years later, the costs to provide that storage have more than halved. But your provider is still working to the terms of their contract, ignoring changes in underlying costs and user expectations.

This can also apply to technology. Your provider might charge you $2000 for a Frame Relay circuit, when they know you a $200 Ethernet circuit would work.

The big cloud providers regularly cut prices and update technology. With smaller providers, you need to make sure this is happening.



## No automated interfaces



Standard requests should have standardised interfaces, allowing automated deployment. You shouldn’t have to wait for a ‘standard 3-day change turnaround’ to make basic changes. Force your provider to give you the tools to get standard changes automatically implemented. Then you can get changes done on your schedule, not theirs.



## Outsourcing rapidly changing/poorly understood environments



If your environment is rapidly changing or poorly understood, no contract will cover all your requirements. Result: Endless change requests, and lots of extra costs. Each “standard turnaround time” for changes adds up to huge delays. This is the biggest challenge for “Your mess for less” providers - it’s still a mess, and it doesn’t cost you less.



## So I should do it all myself?



No, absolutely not. It doesn't make sense to try to do everything yourself. But think carefully about what you're doing, and why you're doing it. Don't blindly walk into something that restricts your business and doesn't deliver the promised benefits.
