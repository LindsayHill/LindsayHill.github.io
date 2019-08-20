---
author: lindsay
date: 2014-11-06 20:03:16+00:00
layout: post
slug: apis-alone-arent-enough
title: APIs Alone Aren't Enough
categories:
- Opinion
tags:
- API
- NFD
- NMS
---

Yes, we know: Your product has an API. Yawn. Sorry for not getting excited. That's just table stakes now. What I'm interested in is the pre-written integrations and code you have that does useful things with that API.

Because sure, an API lets me integrate my various systems however I want. Theoretically. Just the same way that [Bunnings](http://www.bunnings.co.nz/) probably sells me all the pieces I need to build a complete house.

{% include note.html content="Random aside: If your 'open API' requires signing an NDA to view details, then maybe it's not so open after all? " %}

If I'm running a small company staffed by developers, then just giving me an API is acceptable. But in a larger company, or one without developer resources, an API alone isn't enough. I want to see standard, obvious integrations already available, and supported by the vendor.

In this spirit, I'm very pleased to see that [ThousandEyes](http://thousandeyes.com/) now has a [standard integration](https://blog.thousandeyes.com/thousandeyes-pagerduty-integration/) with [PagerDuty](http://www.pagerduty.com/):

> ThousandEyes appears as a partner integration from which you can receive notifications; and, within ThousandEyes we now have a link to easily add alerts to your PagerDuty account.

You can read more at the [ThousandEyes blog](https://blog.thousandeyes.com/thousandeyes-pagerduty-integration/).

This is exactly the sort of obvious integration I like to see offered by vendors. When there are clear links between your product and another popular product, then make it easy to connect them together! I still want the API hooks so I can do my own weird custom integration, but there should be minimal friction for the obvious integrations.

{% include note.html content="Disclosure: ThousandEyes was a sponsor of [Network Field Day 8](http://techfieldday.com/events/nfd8). This meant that they in part paid for my costs to attend that event. This post isn't really about them specifically, but hey, I like to be open about my possible influences." %}
