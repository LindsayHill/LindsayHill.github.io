---
author: lindsay
date: 2014-02-23 18:00:13+00:00
layout: post
slug: solarwinds-dpi
title: SolarWinds DPI - Looks Interesting
categories:
- NMS
tags:
- SolarWinds
---

**[UPDATE 26/6/14]** This code is now available as a [Release Candidate](http://thwack.solarwinds.com/community/solarwinds-community/product-blog/blog/2014/06/25/npm-110-release-candidate-now-available).

SolarWinds talked about their plans for Deep Packet Inspection at [Network Field Day 6](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-6/). That code is now moving into [Beta phase](http://thwack.solarwinds.com/community/solarwinds-community/product-blog/blog/2014/02/11/beta-for-solarwinds-deep-traffic-analysis-now-available):

> With the ability to sniff the wire and analyze packet-traffic, DPI provides real observed network response time (NRT) and application response time (ART.) In addition, DPI has the ability to classify and categorize ~1300 different applications by associated purpose and risk-level.

SNMP on its own can tell you how full your pipes are, but it doesn't tell you much about what's filling those pipes. If you add NetFlow/sFlow, you can find out who and what is filling those pipes. But that doesn't tell you much about how those applications are performing. You can see how many bytes were transferred, but you can't see what the response time was, and you can't work out if delays are caused by the network or the application.

To get that sort of detail, you have to sniff the traffic on the wire, and perform detailed session analysis. There are several products on the market that do this, but they've generally been in the "reassuringly expensive" category, and have not seen widespread deployment. SolarWinds generally prices their products at a more acceptable level for the wider market. I'm going to be keeping a close eye on this one, to see how it stacks up.
