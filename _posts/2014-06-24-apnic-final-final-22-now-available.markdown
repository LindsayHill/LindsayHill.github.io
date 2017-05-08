---
author: lindsay
date: 2014-06-24 06:23:06+00:00
layout: post
slug: apnic-final-final-22-now-available
title: APNIC - final 'final' /22 now available
categories:
- Routing &amp; Switching
tags:
- APNIC
- IPv6
---

[APNIC](https://www.apnic.net/) entered their "[final /8](https://archive.apnic.net/news-archives/2011/final-8/)" phase in April 2011. From that time, new and existing APNIC members could request a maximum of one IPv4 /22 prefix. Once you had requested your final /22, that was it. No more IPv4 addresses for you. Or was it?

APNIC & IANA have been rummaging around down the back of the couch, finding some recovered IPv4 blocks, and they've managed to cobble together a [total of a /11](https://archive.apnic.net/news-archives/2014/apnic-receives-total-of-a-11-from-iana/) to allocate to APNIC.

APNIC has enacted [Proposition 105](https://www.apnic.net/policy/proposals/prop-105), which allows members to request up 1,024 additional addresses, even if they have already received their "final" /22. This was [implemented on May 28, 2014](https://archive.apnic.net/news-archives/2014/additional-ipv4-space-now-available-from-apnic/). Requesting additional space is pretty straightforward - go to [MyAPNIC](https://myapnic.net/), go to Resources -> Resource Request Forms -> IPv4 addresses, and fill in the application form.

You still need to provide justification for the new space, and there's an extra caveat: You must not transfer the space to anyone else for at least 12 months. This is to prevent people flipping them on for an immediate profit. I'm sure there are people setting up shell companies, registering with APNIC, paying the membership fees, just to get a new member allocation of /21, and then planning on selling it. At least now they'll need to sit on it for a little while. At current membership rates vs IPv4 prices, it's a profitable, if morally questionable business to be in.

Application was pretty straightforward, and it was processed pretty quickly. The new block was allocated within a few days. Time to update BGP filters and Geo-location DBs again. Sigh.

(Oh and yes I know: IPv6. I know, I know).
