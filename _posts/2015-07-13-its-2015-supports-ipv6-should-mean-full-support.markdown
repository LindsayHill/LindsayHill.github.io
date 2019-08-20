---
author: lindsay
date: 2015-07-13 04:48:35+00:00
layout: post
slug: its-2015-supports-ipv6-should-mean-full-support
title: 'It''s 2015: ''Supports IPv6'' should mean full support'
categories:
- Routing &amp; Switching
tags:
- IPv6
---

It's 2015. ARIN is [finally out of IPv4 addresses](https://www.arin.net/resources/request/ipv4_countdown.html), more than [20% of Google users in the US are using IPv6](http://6lab.cisco.com/stats/search.php)...and vendors are still doing a half-assed job with IPv6 support. I purchased a new [TP-Link](http://www.tp-link.com.au) Wi-Fi router/modem recently, and it doesn't fully support IPv6. It's not good enough, and I will be returning it.

I purchased the [Archer D5](http://www.tp-link.com.au/products/details/cat-15_Archer-D5.html) "AC1200 Wireless Dual Band Gigabit ADSL2+ Modem Router." The website blurb includes this:

> IPv6 Supported. The next generation of Internet protocol, helping you to future-proof your network.

And the [specifications page](http://www.tp-link.com.au/products/details/cat-15_Archer-D5.html#specifications) says: "IPv6 and IPv4 dual stack."

I checked the documentation for how to configure IPv6. [This FAQ](http://www.tp-link.com.au/faq-857.html) walks through configuring IPv6 on several TP-Link devices. Note that it includes this line "...choose Connection type (Here we just set up PPPoE as an example, if you are not sure, please contact your IPv6 provider)"

In New Zealand, most ADSL services are delivered as PPPoA. The specifications page says this device supports PPPoA. My [ISP](http://www.snap.net.nz/) provides native IPv6 via DHCPv6 PD. So everything should be good to go, right?

Not so much. The Archer D5 does indeed support PPPoA. It also supports IPv6 with DHCPv6 PD. **But** it doesn't support IPv6 with PPPoA. It only supports IPv6 with PPPoE.

I have confirmed this with TP-Link support. They suggested that a future firmware update may resolve this, but they do not have any dates for this. They suggested I could try an IPv6 tunnel broker, but why would I want to use a tunnel when my ISP supports native IPv6?

This is disappointing. I will return the TP-Link device this week. I'll tell them why at [PBTech](http://www.pbtech.co.nz/), but it probably won't get back to the vendor. Anyone got any suggestions for a suitable replacement? I'm looking for these features:

* ADSL2+ modem.
* Dual-radio wireless, preferably with 802.11ac, but 802.11n may be acceptable.
* Full IPv6 support for DHCPv6 PD via PPPoA

Shouldn't be too hard? Maybe I should just stick with the SRX-110, and upgrade the Airport Express to a newer dual-radio version?
