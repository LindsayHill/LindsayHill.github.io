---
author: lindsay
date: 2014-03-29 23:38:18+00:00
layout: post
slug: ios-xr-stuck-snmp-and-sdn
title: 'IOS-XR: Stuck between SNMP and SDN'
categories:
- Routing &amp; Switching
tags:
- cisco
- onePK
- snmp
---

SNMP may be outdated, and is definitely unloved, but it still serves a purpose. We're moving to a new world, with new methods and data structures for interrogating and configuring our network devices. This is a Good Thing, but we're not there yet, and this leaves us stuck in limbo for trying to solve Real World problems today. Here's what I found recently when trying to do some network monitoring with IOS-XR devices.


## Use-Case: Address Pool Monitoring


I need to monitor the current consumption of several address pools, on several Cisco routers. This is trivial to do using SNMP and the [CISCO-LOCAL-POOL-MIB](http://tools.cisco.com/Support/SNMP/do/BrowseMIB.do?local=en&step=2&mibName=CISCO-IP-LOCAL-POOL-MIB). Say what you like about SNMP, but this is a classic use-case for SNMP, and it works well. Simple polling of a few OIDs, and I've quickly created a graph showing my address pool usage, and free space. This is working in production for ASR1000s (running IOS-XE), and it would work well with classic IOS.

We're moving to ASR 9000s, running IOS-XR. These [don't support](http://www.cisco.com/c/en/us/td/docs/routers/asr9000/mib/guide/asr9kmib/asr9kmib3.html) the Local Pool MIB, so I can't poll those values via SNMP. I don't know why they can't be bothered implementing support for a MIB that's been around for many years, but it is what it is. A Cisco SE suggested I look into [onePK](http://www.cisco.com/c/en/us/products/ios-nx-os-software/onepk.html). I like the idea of being down with all the [hip kids](http://keepingitclassless.net/) on that SDN thing. I like the idea of making some API calls to get the data I need, even if it does make a little more work for me to configure my NMS. Let's dig into it.


## Not so Fast (1): Controlled Availability


onePK is in "Controlled Availability". Let's see what happens when we try to [download the SDK](https://developer.cisco.com/fileMedia/download/248b858e-d723-4639-8329-95c995d7a1a4):


> Your account does not have the appropriate access to display the requested page. Please contact your Cisco account representative for further direction.


Sigh. I'm only a CCIE. That means I paid Cisco a lot of money, but it doesn't mean that I get any benefits from Cisco. I'm just an independent guy, I don't have a Cisco account representative. So no download for me. But hang on, [this page](https://developer.cisco.com/site/networking/one/onepk/sdk-and-docs/) lets me "submit a request for consideration to participate in the Controlled Availability phase." Tug the forelock, bow, scrape, please [Uncle John](http://etherealmind.com/network-dictionary-uncle-john/), can I get access? Hilarious that it's supposed to be version 1.1, and it's not yet GA. I think they number versions differently to me. OK, well what happens if I try to [fill in the form](https://developer.cisco.com/form/onePKaccess)?


> Cisco onePK "Controlled Availability" offering is now closed. General Availability coming soon!


OK, let's just ignore that for now - I have heard that GA is supposed to be within days, not months.


## Not so Fast (2): What Version Are You Running?


The [Compatibility Matrix](https://developer.cisco.com/media/onePKGettingStarted-v1-1-0/GUID-33EF791A-2BF2-41F9-B87A-2DE7BAFC8769.html) states that 5.1.1 is required for the ASR 9000. Here's the problem though - that is the very latest image available for those routers. The guidance we received was that 4.3.4 was the recommended version to run in production, unless you really, really needed a feature in the 5.1.x code. The 5.1.x code was not working well for us, and we had to go back to 4.3.x.

But I get that it's early days for onePK, and they can't support all the older code out there. Let's just ignore these issues, and look at the features: Could onePK help us anyway?


## CLI wrapped in an API


Let's look at the [Python API documentation](https://developer.cisco.com/media/onePKPythonAPI-v1-1-0/index.html) for onePK.  It's an API, so my expectation is that there will be some class that represents the type of data structure I'm trying to query, and that I'll be able to find all defined address pools, their configuration, current usage, and available space. Classes named `onep.AddressPool.IPv4Pool` or something like that. Nice defined structures, variables, methods, etc. All the things we would expect of an API.

OK, let's go through the available options: Routing, QoS, Interface Status/Configuration, CDP, syslog, etc...but nothing resembling address pools. What am I left with? [onep.vty](https://developer.cisco.com/media/onePKPythonAPI-v1-1-0/toc-onep.vty-module.html). Sigh. The best I can do is take my CLI command "[show ipv4 local pool](http://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k_r4-3/addr_serv/command/reference/b_ipaddr_cr43asr9k/b_ipaddr_cr42asr9k_chapter_01000.html#wp7784738720)", wrap some API calls around it, and parse the output just as if I was using SSH + Expect.

Net result: Increased development effort, with no benefit - I would still just be screen-scraping CLI output. I'd still have exactly the same issues around trying to turn human-readable information into machine data.

I can appreciate where Cisco is trying to go with developing better programmable access to network devices. I truly believe that it's a good thing. But they're not there yet, so it's a shame that a reasonably modern [NOS](http://en.wikipedia.org/wiki/Network_operating_system) like IOS-XR is stuck in the middle. It doesn't support the traditional methods, nor does it fully support what we would like to do with modern methods. Back to Python + Paramiko for now. I'll take another look when the next onePK version comes out.
