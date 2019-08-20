---
author: lindsay
date: 2013-06-19 10:00:46+00:00
layout: post
slug: hp-discover-thoughts
title: HP Discover Thoughts
categories:
- Opinion
tags:
- hp
---

I attended [HP Discover](http://www.hp.com/go/discover) in Las Vegas this year as a blogger and speaker, courtesy of HP. HP Discover is HP's main technology conference. It's held once a year in Las Vegas, and covers everything that HP does. It's a big conference, with a lot going on, and I only had time to cover a small part of it. This means my take on it may be quite different to another attendee's. I've broken up my thoughts into three key areas - people, vision, and technology. Ultimately they are all related - the right people, with the right vision, will develop the right technology.

{% include tip.html content="Favourite announcement of the week: The next version of IMC will not use client-side Java! HTML5 all the way! This is a big deal - better security, no annoying pop-ups, and much wider client device support." %}

### People

I said the other day that it's all about [the right people]({% post_url 2013-06-13-the-right-people %}). I had access to a range of highly knowledgeable people - developers, architects and those with the ability to set future product direction. The conversations you can have with people who actually write code are worlds apart from those you have with marketers.  The biggest thing I have noticed this year is that HP staff are actively reaching out to people like me, and seem genuinely interested in what I have to say. We'll see just what that turns into, but I appreciate the fact that they are making a genuine effort. A couple of years ago it seemed that HP didn't want to talk about anything beyond the next week, but now people are seeking engagement on products that won't ship until next year. I see this as hugely positive.

One other thing is around executive leadership. It's no secret that HP's had many problems over the last 10-15 years, that stemmed from the Board and CEO.

> Talking to engineers on the ground, people seem happy with what Meg Whitman's doing. There's no great theatrics, no great gestures, or searching for miracle products. Instead she's starting from the basics: Don't \*\*\*\* it up. And people like that.

Certainly some teams are struggling with reduced numbers, but overall there appears to be an appreciation you need funding to deliver the research and development required to sustain this company. The earlier cuts of Mark Hurd went deep, and some areas of the business are still struggling to recover - e.g. I have my doubts about the long-term direction and sustainability of some business units. But that's for another post.

### Vision

Companies don't just make purchasing decisions based upon what you can deliver today - they want to know if you are going to be able to deliver over the next 5 years. So I'm not just interested in what HP can deliver today, but where do they think the market is going, and can they deliver on it? From my discussions with various groups, I felt that there are some excellent ideas and people, but teams still operate too independently. Working together as a whole still seems too hard - easier to focus on making your product better, and not on working well as a unit.

I see some good vision around core infrastructure management. I like HP's direction around monitoring of servers up to the VM level, and network management, integrating SDN, wired and wireless management. This is coming from the hardware side of the business. I see some interesting point solutions from the HP Software group around Service Management. But what I still don't see is any good vision on bringing this together in a cohesive way, in a way that real organisations can implement. I should have one consolidated view, from the underlying hardware up to the services being delivered on top of it. In theory HP should be able to pull this all together, across SaaS, PaaS, IaaS and local infrastructure - but I just don't see the wider vision, and it disappoints me.

### Technology

HP has some interesting things happening on the technology front. Areas I was interested in were SDN & Network Management, and Moonshot systems.

I was in a "Coffee Talk" with two of the key people behind Moonshot. Last year this was in the Lab section of the showroom floor - this year it's very much front and centre, as it's shipping now. The product is an interesting one, but people need to understand that this is about addressing specific use-cases. It is not about general workloads, like a typical x86 system. Strategically it's crucial for HP. The large server consumers - e.g. Facebook, Google - have moved to directly sourcing systems from manufacturers, bypassing HP, Dell, etc. For some reason, Bing has stuck with Dell and HP, but for how long? The large consumers have moved away, and it's unlikely they'll come back. So HP needs to look at other markets. They're targeting specific types of workloads. The key to Moonshot is that it can be adapted to deliver new types of systems that target specific workloads. Critically, they can do this with much shorter turnaround times than normal hardware design, development, testing life cycles.

{% include note.html content=" If HP can be flexible, and quickly adapt to new requirements, they can stay one step ahead of the market, and avoid the margin-destroying march of commoditization." %}

So far I've seen little of how we might integrate Network Management with SDN. Sure, it's great to be able to direct flows around my network - but I still need to get instrumentation of how devices and links are performing, so that we make the right decisions about how to manage those flows. I also believe we're going to see hybrid networks in the enterprise, so we'll still need many of the traditional management approaches - at least for the next 5 years.

Ken Gott presented HP's vision of how they're going to integrate SDN management with IMC, their existing network management product. They intend to have IMC communicate via APIs with HP's SDN controller, and continue to communicate with the underlying network devices via SNMP and CLI. The actual pushing of OpenFlow commands will be done via the SDN controller - not via IMC. But IMC will have full visibility of the network, and will be able to make changes, via the SDN controller. IMC will also be used to manage SDN applications that run on top of the SDN controller.

This will ship later this year. Initially it will only work with HP's SDN controller, but the expressed intention is that it will be able to integrate with other controllers that have open APIs. This will probably be determined by market demand - e.g. if Open Daylight takes off, then we could probably expect to see it supported. This fits in with IMC's broader vision of being open, and working with multiple vendors. I'm pretty excited about this - it's great to see a working (demo) product showing how we might combine management of legacy and SDN networks. I will be writing more on this as I get more details.

### Conclusion

HP is an important company, with a huge influence across the industry. They've had some challenges over the last few years, and the next few years will continue to be difficult. Talking to people at HP, I get the feeling that they feel they may have turned the corner. It's still a lot of uphill, but they feel they're moving in the right direction. They're not swaggering, but there does seem to be a solid underlying confidence. If they can execute well, keep getting the little things right, and avoid the blunders of the past, they might just make it through. I just wish there was a bit more cohesiveness across departments, and broader vision across service management.

{% include note.html content="Disclaimer: HP paid for my flights to HP Discover in Las Vegas, my accommodation, and for some meals. They did not pay me, nor did they specifically ask me to write anything." %}
