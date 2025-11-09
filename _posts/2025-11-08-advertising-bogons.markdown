---
author: lindsay
categories:
- Routing &amp; Switching
date: "2025-11-08 16:00 -07:00"
layout: post
slug: advertising-bogons
tags:
- war stories
title: Advertising Bogons (Or Was I?)
series: 'War Stories'
---

Been a while since I did a "War Stories" post - here's one about a routing policy I screwed up recently. Gave me a fright that I'd really messed something up, but in the end it was no big deal, and it taught me something about who uses route collector info.

## Uh-oh...we're announcing bogons?

While looking at [bgp.he.net/AS32590](https://bgp.he.net/AS32590) for something unrelated, I saw this:

[![announcing bogons](/assets/2025/11/announcing_bogons.png)](/assets/2025/11/announcing_bogons.png)

Investigating more, it tells me this:

[![list of bogons](/assets/2025/11/bogon_prefixes.png)](/assets/2025/11/bogon_prefixes.png)

What the hell is going on? We should **never** be announcing bogon ranges to any peer. I rushed off to check some of our peering sessions, e.g

```
lindsayh@rtr> show route advertising-protocol bgp 86.104.125.69

inet.0: 1009955 destinations, 8974886 routes (1008431 active, 2 holddown, 2770 hidden)
  Prefix		  Nexthop	       MED     Lclpref    AS path
* 155.133.226.0/24        Self                                    I
* 155.133.229.0/24        Self                                    I
* 155.133.250.0/24        Self                                    I
* 162.254.197.0/24        Self                                    I
```

We're just advertising the normal set of prefixes I expect at that site. Defintely not advertising anything unusual to HE. So why do they think we're advertising bogons?

Hmmm...[Cloudflare Radar](https://radar.cloudflare.com/) also says we're announcing junk. Must be a clue there.

## A NOC that responds

I reached out to HE, and their NOC is terrific - they responded very quickly on a Friday evening, even though we're not a paying customer. Kudos to them.

They pointed out that their [Super Looking Glass](https://bgp.he.net/super-lg/) gives you an indication of where they're learning those prefixes from. For example, if you look at [155.133.226.0/24](https://bgp.he.net/super-lg/#155.133.226.0/24?tob=none&mt=include&ma=32590&els=exact), you'll see "Learned from: route-views.sg"

This was the bit I'd missed - [bgp.he.net](https://bgp.he.net) is not just using information from what they see advertised to them. They are also looking at data received by route collector services, such as [RouteViews](https://www.routeviews.org/routeviews/). I had recently added sessions with RouteViews. They wanted to see our full tables, to get another view of what a well-peered edge network sees. I had somewhat carelessly advertised them our full tables...which includes some prefixes that we use internally. We of course strip those out for advertisements to regular peers, but I had reused a policy that should only be used for our internal sessions.

## No Real Harm

No real harm done, since we didn't advertise those prefixes to any "real" peers. Plus of course everyone else is stripping bogons on received prefixes, right? Right?

But it was interesting for me to learn more about how sites like bgp.he.net and radar.cloudflare.com get their data.

{% include series.html %}
