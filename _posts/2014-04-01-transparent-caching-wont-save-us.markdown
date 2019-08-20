---
author: lindsay
date: 2014-04-01 02:23:00+00:00
layout: post
slug: transparent-caching-wont-save-us
title: Transparent Caching Won't Save Us
categories:
- Routing &amp; Switching
tags:
- caching
---

A recent Gigaom article asks: "[Will transparent caching reshape the future of video on the internet](http://gigaom.com/2014/03/14/will-transparent-caching-reshape-the-future-of-video-on-the-internet/)?"

> SUMMARY: Content companies including Viacom and HBO have met with folks from Comcast, Qwilt and others to discuss transparent caching as a way to solve the problem of online video.

The article talks about transparent caching as if it's something new, but it isn't - it's been around for years. Transparent caches have been widely deployed, but they are becoming less effective, and more problematic. I don't see transparent caching solving our problems with managing content delivery.

## What Are Transparent Caches?

Transparent caches observe HTTP traffic passing through the network, storing a copy of downloaded content. Then the next time a user requests that object, it can be served from the local cache, instead of from the remote server. This can save significant amounts of upstream bandwidth, as popular objects only need to be requested once. It can also improve the user experience, by delivering frequently accessed content faster. This is particularly noticeable for low-bandwidth, high-latency links.

Traffic is redirected to Transparent Caches using techniques such as Policy-Based Routing, WCCP, or inline deployment, where all traffic goes through the cache. No end-user configuration is required (that's the 'transparent' part!).

## The Problems with Transparent Caches

I've evaluated some transparent caches recently, and they have a number of issues:

1. **Cost: **They are very expensive to deploy, and they don't have a viable payback time for money saved in bandwidth costs - and that's without even factoring in falling bandwidth costs.
2. **Performance:** Systems like the [Blue Coat CacheFlow](http://www.bluecoat.com/products/cacheflow) top out at around 2Gbps of throughput. That sounded OK a few years ago, but now is looking fairly lame for a hardware appliance. Surely I could get more out of a Virtual Appliance?
3. **Poor Deployment Options: **Blue Coat has the cheek [to say they have](http://www.bluecoat.com/documents/download/8d215e9f-4597-4987-9ba6-9b8dbbb31bd8/71553ee3-7915-4d68-bb67-99538049910a): "...tight integration with existing infrastructure. This includes integration with routers from Cisco and Juniper..." - but when you look into it, their only option is using Policy-Based Routing! Want more than one? You'll need to buy a separate load-balancer for that. Other vendors offer WCCP, but I've heard that charitably described as "[the best broken chair in the dump](https://twitter.com/ajcochenour/statuses/441081903060238336)". Asymmetric traffic is a problem, similar to having stateful firewalls in your network.
4. **Changing nature of content**: There's a growing shift towards SSL, and most of us can't read that. No visibility means no caching. Object size is a problem too - sure we can transparently cache lots of funny cat videos, but how do we do that in a workable manner for multiple sources of movie-size video files?
5. **Lack of Content Integration:** Transparent cache providers may claim they are "intelligent" because they look at real traffic patterns, and detect what should be cached. The problem is that they have to make assumptions about that content, and they sometimes get it wrong, and serve up out of date content. Transparent cache operators will be aware of the need to apply "bypass" rules for specific sites.

## CDNs - Our Only Realistic Option Right Now

Rather than using generic caches, we can use caches provided by Content Delivery Networks (CDNs). These systems host large amounts of content, from one or more content providers. These aren't transparent - through DNS tricks, users are redirected to the content hosted on the CDN edge node closest to the end user. They may not host all content - they can act as a proxy, automatically retrieving content.

CDN Operators such as Akamai charge content providers for using their services. Google also has caching systems - particularly useful for YouTube. Netflix also offers CDN edge nodes to any ISP that wants to host them. The CDN operators don't charge the service provider for these systems - they charge the content providers instead.

ISPs traffic profiles usually show that a few sources - YouTube, Netflix, etc. dominate their overall traffic patterns. If a couple of sources could be delivered from a CDN instead, there's not much cost advantage in transparent caching the rest of the data.

Because the CDN operator manages the content on their systems, and they manage how clients retrieve that content (through DNS, etc), then they are in full control of the delivery. They know that they've cached complete objects, and they can pre-populate content they know will be popular. SSL can still work too, since the CDN knows the content being delivered, and it can tell the client the URL to retrieve content from. No tricky manipulation of traffic flows either. From an ISP's perspective, it's like hosting a web server. No funny path manipulation, and no problems with asymmetric traffic. Typical performance numbers show 30% of traffic can be served up by the CDN caches.

## No Future for Transparent Caches Then?

There are some specific situations where you might want to cache all traffic - e.g. if you're a large New Zealand-based ISP that sells International transit to other ISPs. By transparently caching, you can charge your clients International bandwidth rates, even if some of the content was delivered from a local cache. With that business model, if they moved to hosting CDN nodes, it would only be Domestic traffic to clients, and that's far cheaper. (Unless of course they put their CDN caches into their International VRF, but that would be sneaky, and surely they wouldn't do that, right? Right?)

There is also a possible future where the Transparent caches could somehow have a standardised signalling with the content providers, marking content, and pre-delivering it to the caches. I presume that is what these working groups are talking about. But when you dig into it, it sounds an awful lot like a CDN. The advantage would be that you could host just **one** CDN/Cache within your network, instead of needing to host boxes from Akamai, Google, Netflix, etc. It sounds like it might have promise. But think about the vested interests here, and the difficulties of trying to get everyone to work together. Likely to happen? Not any time soon.

---

### Aside: Bandwidth Caps

I find this part of the article somewhat hilarious:

> Cynics might argue that this fear is manufactured as a way to implement bandwidth caps and justify efforts to raise the price of bandwidth.

I live in a country where bandwidth caps are the norm. In my consulting work, I see the service provider side of the equation. There are real costs associated with provisioning network capacity. If consumption is rising faster than bandwidth costs are falling, what do people expect the price to do? If some users are using dramatically more bandwidth, why should they be subsidised by the rest? ISPs have to pay for peak traffic requirements, but it's difficult to work out a pricing model based upon time/data source that could be easily understood by customers. So ISPs use bandwidth caps as a proxy for true usage-based metering, trying to use market forces to alter behaviour. Some ISPs have an 'uncapped off-peak hours' arrangement where users can download as much as they like during off-peak times, to reflect the available upstream capacity. This is a small improvement. Maybe one day we'll figure out a better model. Just don't think that you can get a constant 20Mbps, 24x7 for $75/month.
