---
author: lindsay
date: 2013-11-01 17:00:21+00:00
layout: post
slug: openness-isnt-just-about-code
title: Openness Isn't Just About Code
categories:
- Opinion
tags:
- sales
- SolarWinds
---

When talking about "Open" in the context of technology, most people think about Open Source, Open APIs, Open documentation, etc. But there's another facet too: being open about where your business is going, what it's thinking about, and what it's **not** going to do. Open Roadmaps, if you like. Some businesses are remarkably open, others won't even tell you what's happening next week.


## The Case for Open: SolarWinds


http://youtu.be/MTIzdyaAIgA

Watching the SolarWinds presentation at [Network Field Day 6](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-6/), I was reminded of just how open they are about their upcoming products, their plans, where they have problems, what they want to fix/deliver, and what they won't be doing. They [won't release full roadmaps](http://thwack.solarwinds.com/community/solarwinds-community/product-blog/blog/2010/02/10/fear-and-loathing-of-roadmaps-why-your-pm-won-t-give-you-a-date), but they cover a lot of information about upcoming new features in their "[What We Are Working On](http://thwack.solarwinds.com/thread/43025)" posts on [Thwack](http://thwack.solarwinds.com). The open discussions they have had at [many](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-5/)[ Tech Field Day](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-3/) [events](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-1/) give a fascinating insight into their thinking.

As a regular engineer, I can easily access this information, and I can use that information to decide if SolarWinds' direction aligns with where I see my company going. Of course, there are some that [disagree with that direction](https://twitter.com/joshobrien77), but that's OK - I think it's better to realise that early on, rather than face years of niggles, never quite getting what you want.


## The Case for Closed: HP


{% include note.html content="NB I'm not specifically picking on HP here, it's just that much of my recent experience is with HP. Plenty of other companies are difficult to deal with - e.g. Check Point" %}


HP can be very difficult to get useful future information from. A typical frustrating example is when I log a defect. My case gets accepted, and I receive notification that yes, this is a defect. Months later, I get notification to say "Your defect has been accepted, and will be reviewed for inclusion in a future release." No indication of when that future release might come out. Within 24 hours, a full patch is released, containing a fix for my defect. I have had similar situations where a temporary hotfix was provided to me, and support said they had no idea when the next fully supported patch would be. The patch, including my fix, was released 3 days later.

Releasing a full patch does not happen overnight for HP Software products. How much would it have hurt them to have told me about things happening more than 24 hours ahead? But they're a big company, with too many lawyers, and too many past lawsuits. Any revenue I might represent is less than a rounding error. If I was bringing tens of millions to the table, perhaps they would be more forthcoming.

Instead, I'm left to read the tea-leaves, and [draw my own conclusions]({% post_url 2013-07-08-abandoned-operations-manager %}).

## Why Not Tell?

There are certainly very legitimate reasons for not telling the whole world everything. Some reasons include:


  * Accounting Practices. Apparently if you tell me you're thinking about doing something in 2 years' time, and then you don't do it, I can sue you, and you may have to restate your earnings. Or some such nonsense, even if you told me "Hey, this is just what we're thinking". That's why everyone layers themselves up in worthless NDAs before they'll tell you anything vaguely interesting.

  * You don't want your competitors to know. There's some merit in this - if you're planning to release a "Cisco-killer" next year, then you probably don't want to give the 800-lb gorilla a headstart on squashing you like the fly you are.

  * Because you don't know. I'm not entirely sure what I'm up to next week, so I can understand it if a company is struggling to narrow down where their product will be in 3 years.

  * Pre-announcing things too soon, when you don't have anything shipping can look pretty bad - you don't want to [bring down the wrath of Ivan](http://blog.ipspace.net/2013/05/interop-product-launch-craze.html).


## Implications for Purchasing Decisions


If you're big enough, it doesn't matter: Almost all large vendors will open their kimonos to you, and you can make an informed decision. Hell, if you're really big, you can change that product direction. But if you're like most of us, it can be very difficult to find out just what the future product plans are. The problem is that you're not just buying a product for what it can do today - you're buying into the vendor vision for the next 5 years, and expect the product to develop along with you. Often we end up having to take a gut-feel stab at it, and hope that we've made the right call. No-one wants to be the guy that made the big commitment to [Itanium](http://www.intel.com/content/www/us/en/processors/itanium/itanium-processor-9000-sequence.html), or [Cisco MARS](http://www.cisco.com/en/US/products/ps6241/index.html), or [Microsoft Zune](http://en.wikipedia.org/wiki/Zune).
