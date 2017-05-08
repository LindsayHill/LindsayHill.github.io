---
author: lindsay
date: 2015-03-22 07:54:40+00:00
layout: post
slug: dnssec-moving-the-needle
title: DNSSEC - Moving the Needle
categories:
- Security
tags:
- DNS
- security
---

The New Zealand ISP market is dominated by Spark, Vodafone & CallPus/Orcon. A side effect of this is that if one player does the Right Thing™, it really moves the needle. Recently, [Spark](http://www.spark.co.nz/) has done the Right Thing with [DNSSEC](https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions).

DNSSEC takeup has been low with New Zealand ISPs. The APNIC stats indicated that around 5% of users were using DNS resolvers that had DNSSEC validation capabilities. But in December 2014, [that number jumped](http://stats.labs.apnic.net/dnssec/NZ?c=NZ&x=1&g=1&r=1&w=1&g=0) to ~15%:

[![dnssec_nz_stats](/assets/2015/03/dnssec_nz_stats.png)](/assets/2015/03/dnssec_nz_stats.png)

It turns out this is because Spark has enabled DNSSEC validation on some of their resolvers. NZRS have [done some analysis](https://nzrs.net.nz/content/dnssec-validation-spark-nz), and found that Spark turned on 4 new resolvers that do DNSSEC validation:

![](https://nzrs.net.nz/sites/default/files/imagecache/dnssec_chart/images/Spark-All-Traffic.png)

They're still running their old resolvers, so right now it's hit & miss for their customers. But it's a great start, and presumably they'll upgrade the remaining systems soon.

So [Vodafone](http://www.vodafone.co.nz/), [CallPlus](http://www.callplus.net.nz/), [Snap](http://www.snap.net.nz/), [Trustpower](http://www.trustpower.co.nz/)...when are you going to take customer security seriously too? And Spark...how long until DNSSEC is enabled for all your resolvers?

And please, no arguments about "we're not sure if it will work." Google has been doing it since [March 2013](http://www.theregister.co.uk/2013/03/20/google_adds_dnssec_validation/)...who do you think processes more DNS requests per day? Google, or your ISP?
