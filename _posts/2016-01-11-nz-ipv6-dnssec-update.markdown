---
author: lindsay
date: 2016-01-11 06:33:01+00:00
layout: post
slug: nz-ipv6-dnssec-update
title: NZ IPv6 & DNSSEC Update
categories:
- Routing &amp; Switching
tags:
- IPv6
---

A year ago I published a table of [New Zealand ISP IPv6 support]({% post_url 2015-01-30-ipv6-availability-in-new-zealand %}). At the time support was fairly poor. I'm pleased to report that things have gotten better over the last year. There has also been a very pleasing uptick in DNSSEC support.


## IPv6 Changes


The big movers here are [Trustpower](http://stats.labs.apnic.net/ipv6/AS55850) & [Orcon](http://stats.labs.apnic.net/ipv6/AS17746), who have both enabled IPv6 by default for their users. So now we have the two largest ISPs still only offering IPv4, but all of the next tier of ISPs are offering IPv6. New Zealand has a flexible ISP market, and almost all consumers can change provider quickly & easily. This means that IPv6 is effectively available for all who want it.

[![NZ-IPV6](/assets/2016/01/NZ-IPV6.png)](http://stats.labs.apnic.net/ipv6/NZ)

{:.image-caption}
New Zealand IPv6 Availability - Click image to see APNIC data

The numbers are still small, but we can see a move upwards towards the end of the year when Orcon & Trustpower enabled IPv6. Many legacy home routers have IPv6 disabled, but as these get replaced/reconfigured, I expect to see a steady increase in IPv6 uptake across those ISPs.

The two market leaders - [Spark](http://www.spark.co.nz/) & [Vodafone](http://www.vodafone.co.nz/) still only offer broken promises. In 2014 Vodafone implied it was not far away: "[I can promise you well well before 2018](http://www.geekzone.co.nz/forums.asp?forumid=40&topicid=154762&page_no=1#1171008)." But I'm not holding my breath.


## DNSSEC - Well done Spark!


DNSSEC validation numbers look much better:

[![NZ-DNSSEC](/assets/2016/01/NZ-DNSSEC.png)](http://stats.labs.apnic.net/dnssec/NZ)

{:.image-caption}
New Zealand DNSSEC Validation - click image to see APNIC data

We see a big jump here, with around half of all DNS queries from NZ going through resolvers with DNSSEC validation enabled. This jump is almost entirely due to Spark changing their policy. Smaller ISPs such as [Trustpower](http://stats.labs.apnic.net/dnssec/AS55850?c=NZ&g=1&w=1&x=1) have also recently enabled DNSSEC validation.

Vodafone continues to drag the chain. No IPv6, no DNSSEC. It's a bit embarrassing that their AS is described in whois as "[VODAFONE-NZ-NGN-AS](https://wq.apnic.net/whois-search/static/search.html?query=AS7657)." Are you allowed to describe your network as "Next Generation" when you don't support basic services such as IPv6 and DNSSEC? There should be rules about that sort of thing.

The good news is that consumers now have a range of viable suppliers for these services. Changing ISP is trivial, and I highly recommend that you change supplier if you're not getting the services you want. If enough people move, maybe the laggards will finally get the message.
