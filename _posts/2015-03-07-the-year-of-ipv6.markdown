---
author: lindsay
date: 2015-03-07 04:18:49+00:00
layout: post
slug: the-year-of-ipv6
title: The Year of IPv6?
categories:
- Routing &amp; Switching
tags:
- IPv6
---

IPv6 adoption has been slow. But I think it’s reaching a tipping point. I’m very close to calling 2015 “The year of IPv6.” There’s plenty of people who won’t believe me, but the statistics are very interesting. You need to keep a close on eye on what the data is saying.

Recently I asked the question “What percentage of Internet traffic needs to be IPv6 for you to consider IPv6 to be mainstream/arrived/the year of IPv6?”

[@bobbobob](https://twitter.com/bobbobob) had the best answer for when IPv6 can be considered ‘mainstream’:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/northlandboy">@northlandboy</a> When I see criminal minds, or law and order using a poorly faked IPV6 address when they&#39;re &#39;hacking&#39;, I&#39;ll say it&#39;s arrived.</p>&mdash; Rabbit Sultan (@bobbobob) <a href="https://twitter.com/bobbobob/status/569076703840083968">February 21, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

But [@icemarkom](https://twitter.com/icemarkom) was probably technically correct with this answer:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/northlandboy">@northlandboy</a> More than 50%.</p>&mdash; Marko Milivojevic (@icemarkom) <a href="https://twitter.com/icemarkom/status/568895795950268417">February 20, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

So how far away is that? It’s tough trying to measure IPv6 adoption. Traffic patterns are region- & user-specific. The services that Chinese users access are different to those that a New Zealand business users. Traffic is often concentrated with a few ISPs and/or a few big services (Google, Facebook, Twitter, etc).

I like to use the [Google IPv6 statistics](http://www.google.com/intl/en/ipv6/statistics.html) as a proxy for measuring IPv6 deployment. These stats show the percentage of Google traffic served up via IPv6 since September 2008. They also show current per-country IPv6 usage statistics. If we assume that most users (apart from China & Russia) use Google services, then I think that these stats serve as a good indicator for how widely available IPv6 is to end-users.

[![Google IPv6 Stats](/assets/2015/03/ipv6_google.png)](/assets/2015/03/ipv6_google.png)

I recommend that you spend some time playing around with these statistics. You might be surprised. Those that still mutter about IPv6 traffic being negligible are wrong. The key is in the growth rate. Overall usage in March 2013 was peaking around 1.2%. By the start of March 2014, it was ~3.15%. As at March 2015 it is peaking at over 6%.

The other very interesting bit is the per-country data. My own country is pretty pathetic at < 1%. But the US is at around 14%, and Belgium is over 30%.

I haven’t done any detailed statistical analysis of this info. But just drawing a rough line, we could assume a doubling of IPv6 traffic levels each year. How long does that take for IPv6 to become the majority of Google traffic? In Belgium, that could be later this year. In the US, it’s maybe two years away.

If people are still telling you that IPv6 isn’t happening, point them to the data. They might be surprised. If they’re waiting until 10% of their users have IPv6, they might be surprised to find that they’re already at that level. I expect that Google global IPv6 traffic will go over 10% this year, but it’s already well above that in some geographies.

Of course, these stats don’t tell us what percentage of services are available. But since Google, Facebook and Netflix are all available via IPv6, it’s easy for an ISP to see a huge chunk of their traffic using IPv6. We should also see more websites available via v6, as people switch to services like CloudFlare.

## Aside: More proof that Enterprise tech is lagging

Take a close look at the Google [IPv6 adoption traffic stats](http://www.google.com/intl/en/ipv6/statistics.html#tab=ipv6-adoption). At first glance they show a spiky nature, moving up and down around a line. But zoom in closer, and look at the stats:

[![IPv6 Weekend Traffic](/assets/2015/03/ipv6_weekends.png)](/assets/2015/03/ipv6_weekends.png)

Look how the traffic shows a clear increase at the weekends. Current typical weekday numbers are ~4.7%, while weekend traffic is ~5.7%. The conclusion I draw from this is that people are more likely to be using IPv6 at home than at the office. Yet again, Enterprise technology is [lagging behind consumer tech](http://etherealmind.com/blessay-enterprise-comes-last-technology-innovation/). Who do we thank for that? ITIL or the GFC?
