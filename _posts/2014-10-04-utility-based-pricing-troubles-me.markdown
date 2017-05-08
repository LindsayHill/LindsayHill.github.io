---
author: lindsay
date: 2014-10-04 08:15:06+00:00
layout: post
slug: utility-based-pricing-troubles-me
title: Utility-Based Pricing Troubles Me
categories:
- Opinion
tags:
- costs
- design
---

Utility, or Consumption-Based pricing models offer an interesting way of matching costs to revenues. But if they’re not managed well, customer costs could blow out just trying to keep the lights on. We've come to expect rapidly declining hardware prices. Have vendors realised their utility prices need to decline at a similar rate?

I’ve been doing more architecture work over the last twelve months, and this has changed some of my thinking about technology. Previously I was only really interested in speeds & feeds, and technical capabilities. Scaling was only about how to add capacity - not what it would cost. When I looked at costs, it was just to shake my head at the ridiculous prices charged for things like a second power supply.

But now I find myself interested in things like cost curves, and trying to figure out how my costs will change as demand changes. The ideal is for their to be a clear relationship between costs & revenue, hopefully with costs growing at a slower rate than demand (and revenue).

Previously we had high upfront costs to buy hardware and software, and we aimed to amortise it over the life of the service. Our costs would grow in steps, slightly ahead of demand. When we needed more capacity, we’d buy more (or bigger) systems. That is now starting to change. In part due to the rise of virtual appliances, vendors are starting to offer consumption-based billing. This might come in the form of hourly/daily/monthly charges based upon usage, or licenses to unlock throughput - e.g. See Cisco’s [ISR 4000 series](http://lostintransit.se/2014/10/04/cisco-adds-new-routers-in-the-isr-4000-family/). Matching costs to demand sounds good, seductive even. But these billing models trouble me.



## Hardware gets cheaper…but we use more of it



Historically we would buy systems, and run them at whatever capacity they were rated at. The price of the gear didn’t change depending on what utilisation we ran it at. The best we could hope for was a discount on standby gear. We paid an up-front capex cost, and on-going support costs. But the price didn’t vary based on how we used it.

Hardware capacity grew rapidly, following Moore’s Law. We knew we could upgrade our hardware in a few years, and the new boxes would be much more powerful. The problem was that our requirements tended to grow in line with capacity. Storage is a good example - hard drive capacity keeps getting cheaper, but we keep storing more stuff. So our total spend stayed about the same, just to keep the lights on.

Customers know this too. They expect continuing increases in capacity, for the same total spend - e.g. In the New Zealand ISP market, standard prices stay about the same, but bandwidth caps keep going up.



## Incremental Growth vs Step Changes



Consider a log management system where you pay based upon volume of data stored. There’s two ways that this log volume goes up: Step changes, and Incremental growth. Step changes occur when you add new data sources. This should be associated with either increased revenue, or increased value from the log management system. But you also have incremental growth, just keeping the lights on. You can easily double your log volumes in 12 months…did your revenues double? Probably not. So it makes it hard to ask the boss for double the budget, right?



## Even small, regular price cuts aren’t enough



AWS provides another example - they’re well-known for offering utility-based pricing. They’ve also been very vocal about regular price cuts. That should mean that the total spend required to meet organic growth should remain fairly flat. But it turns out they weren’t dropping prices as fast as they might have done. Google’s move [earlier this year](http://www.theregister.co.uk/2014/03/26/amazon_price_cut/) highlighted this, and re-adjusted the cost curve. Strong competition has forced all players into line here. What would have happened if that competition wasn’t there?



## Watch your pricing models



Consumption-based billing definitely has a place. It can be a great way to try to get your costs to track well against your revenues. But remember that what your baseline load will constantly increase at a greater rate than your revenues. Make sure your that you’re telling your vendors that you expect to see continuing regular declines in per-unit costs. If they can’t give you that, look for alternative vendors or pricing models.
