---
author: lindsay
date: 2013-09-29 17:00:46+00:00
layout: post
slug: splunk-overview
title: Splunk Overview
categories:
- NMS
tags:
- logs
- Splunk
---

This is a quick overview of Splunk - what it is, how it works, what makes it good, and how you might use it. I like Splunk, and I think most engineers should have at least a passing familiarity with it.

## What is Splunk?

To quote [Splunk themselves](http://www.splunk.com/product):

> Splunk Enterprise is a fully featured, powerful platform for collecting, searching, monitoring and analyzing machine data. Splunk Enterprise is easy to deploy and use. It turns machine data into rapid visibility, insight and intelligence.

Basically Splunk takes in all of your text-based log data, and provides you an easy way to search through it. It started out as "Google for your logs", but it's become far more than that, as capabilities have been added. Now you can pull in all sorts of data, and perform all kinds of interesting statistical analysis on it, and present it in a variety of formats. You can simply search for specific patterns, or you can generate all manner of graphical reports:
[![](https://www.splunk.com/content/dam/splunk2/images/products/product-tour/splunk-enterprise/tour-search-investigate.png)](https://www.splunk.com/en_us/products/splunk-enterprise.html)
[![](https://www.splunk.com/content/dam/splunk2/images/products/product-tour/splunk-enterprise/05-correlate-complex-events-product-tour.png)](https://www.splunk.com/en_us/products/splunk-enterprise.html)


## How does it work?

There's three key components to a Splunk installation:

  * **Forwarder:** Forwarders forward data to remote indexers. They may also index it locally. Data can be taken in via many different methods - monitoring log files, syslog, TCP, WMI, running scripts, querying a database, etc. Data can be forwarded to multiple indexers, or split.

  * **Indexer:** This is the heart of the Splunk setup - it stores and indexes data, and responds to search requests. This is their "Magic Sauce" - they use some [fairly interesting techniques with MapReduce](http://www.splunk.com/web_assets/pdfs/secure/Splunk_and_MapReduce.pdf) to store and process large amounts of unstructured data, and still be able to return timely search results.

  * **Search Head:** This is the main front-end, commonly accessed via the Splunk web interface. Search Heads can run searches across multiple Indexers, so you can scale out.

These components can be combined, or distributed for full flexibility. Recent development work has been around SDKs and APIs to provide more methods for inserting data, and for running queries. Using the API allows you to integrate Splunk into your other applications, without using the regular Splunk web interface. The Splunk team are quite happy about that - they see their value in the indexing and searching, not in the presentation.


## What makes it so good?


> Splunk...4-5 years ago...it just pulls a lot of logs in, tells you what happened in a specific time span. Today it's really a platform, that you build apps on top of.

(Paraphrasing Denny LeCompte, SolarWinds, [NFD6](http://techfieldday.com/appearance/solarwinds-presents-at-networking-field-day-6/).)
Applications and integrations are at the heart of what makes Splunk useful. Once you've built up a set of useful dashboards, data definitions, saved searches and reports, they can easily be distributed as applications. Many vendors and independent developers have developed useful applications, and distribute them at [apps.splunk.com](http://apps.splunk.com). The recent development work around SDKs and APIs allows deeper integration with other applications. The combination of these two factors makes Splunk more of a platform, and not simply a standalone product. It is this that provides enormous value to customers. Look at examples like the "[Splunk for ServiceNow](http://apps.splunk.com/app/1228)" application - this lets you search ServiceNow data as another data stream - or you can go the other way and automatically insert tickets into ServiceNow in response to events.

The other things that make Splunk so good are its flexibility and speed. By breaking the application into components, Enterprises have a lot of flexibility in how it is deployed - e.g. all nodes at a site could use forwarders to monitor logfiles, which then forward to a single forwarder, which re-forwards the data to a central Splunk instance. Or the data could stay at each site, and a single search head could query multiple sites at once.

Speed comes from the underlying technology, and the ability to scale out. It's certainly not always super-fast: But it's a vast amount better at querying unstructured data than traditional databases. You do need to commit a reasonable amount of resources to make it fast, but at least it's relatively simple to do so.


## Common Use-cases


  * **Operational Monitoring:** automated alerting, investigating incidents across multiple logs, capacity planning. NOC-style dashboards are easy to create, to show current activity.

  * **Security:** I've seen it used to help comply with the PCI-DSS "Daily Log Review" requirement. A classic dashboard might show all security "incidents", or perhaps all log patterns that do NOT match known good patterns.

  * **Customer Behaviour Analysis:** You might pull together data from a variety of sources, in an attempt to understand customer buying patterns, e.g. look at how many customers are logging in each day, what items are most commonly added to baskets, and how long a user spends on your site before purchasing.

Note that these are not exclusive uses - you could take in the same data, but each group could have a unique set of views, reports and saved searches against that data. Combining multiple use-cases is often the best way to use Splunk, and it can help you get funding for your project.


## Why Might I **NOT **Use Splunk?


There's two main reasons for not using Splunk:


  1. The price is well into the "reassuringly expensive" category, and if you have large daily data volumes, it can become prohibitively expensive. It's licensed on the basis of daily log volumes. NB: You can burst above your daily limit up to 5 times per month before it will stop working, so you can cope with short spikes in demand. You can also run a free instance if you're using less than 500MB/day, and don't need authentication.

  2. You don't want to run your own infrastructure, but would prefer to access it as a service. Splunk does offer [Splunk Storm,](http://www.splunkstorm.com/) a cloud-based version of Splunk, but it's feature limited, and development seems a little slow.

This is not to say that you should only use Splunk. Not at all. There are many other good products on the market, and others may well fit your needs better. You might find other products have better integrations with other systems you use - e.g. if you use the HP BSM suite, then [ArcSight](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1340712#.UjQpqWRgZVc) might be an excellent choice based upon its integration capabilities. Splunk's general-purpose nature might appeal to you, or you might need something that is tightly targeted towards a specific need.


## Wrapping Up

I'm a fan of Splunk. I think it's a fantastic product, that has an enormous number of uses. You should definitely evaluate it when looking for log management tools. The only major downside is the price - if you really need it, hopefully you can come up with a business case that stacks up.
