---
author: lindsay
date: 2014-07-23 07:39:06+00:00
layout: post
slug: network-gear-pricing-software-vs-hardware
title: Network Gear Pricing - Software vs Hardware
categories:
- Opinion
tags:
- cisco
- sales
---

Network equipment pricing has traditionally been based around hardware, even though most of the cost comes from the software. Trends such as bare-metal switching will clarify this cost/price relationship. But are we already seeing software showing up as a clear component of the prices of network gear?

## Pricing vs Costs: Hidden Software Charges

The always excellent [@mbushong](https://twitter.com/mbushong) recently wrote about [bare metal switching](http://www.plexxi.com/2014/07/bare-metal-switching-pricing-impacts-resellers/), pricing, and the impact this has on resellers:

> For anyone looking at the broader implications of bare metal switching, the first thing to understand is networking’s pricing model. While the vast majority of investment (as much as 90% of R&D at large companies) is on the software, the hardware accounts for the lion’s share of pricing.

But when I thought about some recent quotes I've seen, software licensing was a significant part of the total price. So I thought I'd do a quick breakdown of the percentages of software vs hardware costs as part of the overall price. This should show how much of the software cost is "hidden" vs broken out.

## Example Pricing Mix


The quotes I had were from a company we'll call "Cadillac," covering routing and switching. I can't publish the exact prices - you'd laugh at how much we have to pay. But I can show the hardware vs software percentages:

|---------+----------+----------|
| Device | Hardware % | Software %
|---------+----------+----------|
| Router - BNG | 67% | 33% |
| Router - Edge | 80% | 20% |
| Switch - Advanced | 65% | 35% [^1] |
| Switch - Basic | 100% | 0% |
|---------+----------+----------|

[^1]: This includes a license charge to unlock hardware features. Is that hardware or software pricing?

Hardware costs include SFPs, so there's no [hidden tax](http://etherealmind.com/rant-extreme-charges-license-fee-using-oem-sfps-limits-bandwidth/) there. I have not included support costs - those tend to be weighted towards software.

If you're using low-end switches, and only running the base feature set, then yes, the pricing is heavily skewed towards hardware. But once you start adding advanced routing/switching features, software starts clearly showing up in the price. Some specific features play a huge part - e.g. BNG capabilities.

Even with those features broken out, we can still see that much of the software investment is hidden in the hardware price. Then again, we're not buying just hardware: We're buying a solution, and we expect it to have some switching & routing capabilities out of the box. I would not be impressed if you wanted to sell me some custom hardware that couldn't do ANYTHING without paying extra software licensing costs. Open hardware is obviously different - you can sell me a blank Intel server, since I have a wide choice of software I can install on it.


## Other Vendors


This will look very different if you're ordering equipment from "Jeep", or "Studebaker-Packard." They use different licensing structures. They may license all features by default, hiding the software cost in hardware pricing. Data-center gear is priced quite differently to SP gear too. Some vendors offer "pay as you grow" options, unlocking hardware capabilities as traffic volumes grow. Sadly, 2x traffic doesn't mean 2x revenue these days.


## SDN/Bare-Metal to Complicate Matters?


I expect to see a good uptake of bare metal switching in larger companies, although I think it will be slow amongst small-medium Enterprises. When the cost of the hardware is clearly broken out from the software, we should get significant margin pressure.

But networking companies won't want to miss out on the margin. I expect to see flat pricing for hardware, and complex schemes for pricing the SDN applications that we perceive as being further up the value chain.


> Where there's mystery, there's margin.

I guess we should be careful what we wish for.
