---
author: lindsay
date: 2016-09-25 23:55:33+00:00
layout: post
slug: when-the-ipv6-data-changes-so-should-your-opinion
title: When the IPv6 Data Changes, so Should Your Opinion
categories:
- Opinion
tags:
- IPv6
---

Sky UK recently [completed their rollout](https://corporate.sky.com/media-centre/news-page/2016/sky-completes-roll-out-of-ipv6-becoming-the-first-major-uk-internet-provider-to-future-proof-its-service-for-customers) of IPv6. The uptake statistics are quite remarkable. If you think that people don't have IPv6-capable devices, or that their home routers can't handle IPv6...you really need to look at the data, and re-think your opinion.

APNIC has a long-running program collecting data on IPv6 client capability/preference by country and ASN. This graph shows the data for [Great Britain](http://stats.labs.apnic.net/ipv6/GB):

[![ipv6_gb](/assets/2016/09/ipv6_gb.png)](http://stats.labs.apnic.net/ipv6/GB)

So a year ago we had ~5% takeup, and now it's 20-25%. And here's the reason for that big jump from April this year - this graph shows the [data for AS5607](http://stats.labs.apnic.net/ipv6/AS5607?c=GB&p=1&v=1&w=1&x=1), BSkyB:

[![ipv6_bskyb](/assets/2016/09/ipv6_bskyb.png)](http://stats.labs.apnic.net/ipv6/AS5607?c=GB&p=1&v=1&w=1&x=1)

So > 80% of clients on the BSkyB network are IPv6-capable.

If someone tells you that people don't have IPv6-capable devices, or routers: the data does not back that up. A few years ago that may have been true, but people don't access the Internet using Windows XP desktops anymore: They use iOS and Android mobile devices. These have short replacement lifecycles, so people tend to be running newer versions. These are capable of using IPv6, and will prefer it if it is available.

The other related trend is that people have more wireless devices at home, and they have faster connections to the Internet than they used to. When people had 1 or 2 devices using a 10Mbps ADSL link, their local wireless performance didn't matter much.

Now they have > 10 devices, and > 100Mbps links. This created demand to upgrade home routers. ISPs offer better devices for 'free', and people are buying their own. These newer devices support IPv6. As ISPs start enabling IPv6 at their edge, everything else is in place.



## Why does it matter?



You could still look at the overall uptake (global peak ~13% of Google users) and say "meh, doesn't matter to me."

Or you could look at the country and user-specific stats - e.g. > 50% of mobile users in the US use IPv6. If you're designing an application that targets those user bases, it makes sense to use [IPv6 where possible,](http://blog.ipspace.net/2016/07/and-this-is-how-you-build-ipv6-only.html) and translate to IPv4 at the edge. That way you'll be doing less protocol translation than going the other way.

Of course, if your target user base is on-site users of Oracle ERP systems...well, who cares? Enterprise is lagging further behind in technology.
