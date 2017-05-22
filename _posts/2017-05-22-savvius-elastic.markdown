---
author: lindsay
date: 2017-05-22
layout: post
slug: savvius-elastic
title: Savvius Insight and the use of Elastic
categories:
- NMS
tags:
- Savvius
- Splunk
---

Last week Savvius [announced](https://www.savvius.com/company/news/press_release/2017-05-16) upgraded versions of its [Insight network visibility appliances](https://www.savvius.com/products/network_visibility_performance_diagnostics/savvius_insight). These have the usual performance and capacity increases you'd expect, and fill a nice gap in the market.

But the bit that was most interesting to me was the use of an on-board [Elastic](https://www.elastic.co) stack, with pre-built Kibana dashboards for visualizing network data, e.g.:

![Savvius Insight Kibana Dashboard](/assets/2017/05/savvius_insight_dashboard.png)

Historically the only way we could realistically create these sorts of dashboards and systems was using [Splunk]({% post_url 2013-09-29-splunk-overview %}). I'm a big fan of Splunk, but it has a problem: Cost. Especially if you're trying to analyze large volumes of network data. You might be able to make Splunk pricing work for application data, but network data volumes are often just too large.

Savvius has previously included a Splunk forwarder, to make it easier to get data from their systems into Splunk. But Elastic has reached the point where Splunk is no longer needed. It's viable for companies like Savvius to ship with a built-in Elastic stack setup.

There's nothing stopping people centralizing the data either. You can modify the setup on the Insight appliance to send data to a central Elastic setup, and you can copy the Kibana dashboards, and create your own versions. No more Splunk license constraints: now it's just a matter of how much central storage you can afford, and how much you need.

> The Insight appliances are running Linux under the hood. Obviously Savvius can't offer full support for whatever you might do at a Linux level, but I liked that they had a pragmatic attitude to it. It's your box, you've got the power, make changes at that level if you need to.

I'm very pleased to see the continuing evolution of log &amp; data management tools, going from high-value novelty to commodity item. One less excuse for the foot-draggers who still don't do centralised logging.

> Aside: Am I the only one who finds it amazing that I can order devices like this via [Amazon Prime](https://www.amazon.com/Savvius-Insight-Network-Monitoring-Appliance/dp/B016APSVP4/ref=sr_1_3?ie=UTF8&qid=1495418623&sr=8-3&keywords=savvius)? We've come a long way from the traditional people-intensive, slow-moving sales process. 
