---
author: lindsay
date: 2014-12-24 05:14:34+00:00
layout: post
slug: resources-for-learning-hp-comware
title: Resources for learning HP Comware
categories:
- Study
tags:
- hp
- VSR
---

HP is making more resources available to help with learning Comware. They've added free labs and courses to the already published simulators and virtual routers. This is a good resource for those looking to get started with Comware.

## HP Network Simulator (HNS, aka Simware)

HP's [Network Simulator](https://www.hpe.com/us/en/networking/products/simulator/index.aspx) (HNS) is a modelling tool for simulating HP Comware networks. It includes Layer-2 functionality, and lets you test things like LACP & IRF. I found it [too slow]({% post_url 2014-02-24-hp-simware-comware-os-simulator %}) when I first tried it, but this has improved significantly with current versions. It is free to download.

HP has now started publishing simple labs you can work through with HNS:

* [Lab 1 - Basic management](https://www.hpe.com/h20195/v2/getdocument.aspx?docname=4AA5-5630ENW)
* [Lab 2 - Port and link management](https://www.hpe.com/v2/getdocument.aspx?docname=4AA5-5631ENW)
* [Lab 3 - Port-based and protocol-based VLANs](https://www.hpe.com/h20195/v2/getdocument.aspx?docname=c04473805)

These are short labs that cover HNS setup, and device configuration. Quick and easy, they show how to use the tool, and give you a taste of Comware configuration. They've also released a free [1-hour online course](http://www.brainshark.com/HP-ESSN/vu?pi=zGlz14Ff6mzL7Caz0) that goes through how to use HNS.

{% include note.html content="Interestingly, the course is narrated by [Natalie Timms](http://natalietimms.com/), formerly of the CCIE Security Program. She's popped up a [couple of times](http://packetpushers.net/podcast/podcasts/show-67-ccie-security-track-update-with-natalie-timms-program-manager/) on [Packet Pushers](http://packetpushers.net/) too." %}

## VSR1000

I've covered the [HP VSR1000 previously]({% post_url 2013-10-14-hp-vsr1000-getting-started %}). This is a virtual router that runs on VMware ESXi, KVM, etc. It's similar to [Cisco's CSR1000v](http://www.fryguy.net/2013/12/27/cisco-csr1000v-for-home-labs/), or [Brocade's Vyatta](http://www.brocade.com/products/all/network-functions-virtualization/product-details/5400-vrouter/index.page). You can license it to unlock higher performance, but it's not a problem in a lab.

Because it's just a VM to spin up, I find this quicker & easier to work with than using HNS. It's Layer-3 only, but that's usually fine for whatever I'm trying to do.

HP has released a hands-on lab guide for the VSR1000. Go to [here](https://www.hpe.com/us/en/networking/products/routers/HP_VSR1000_Virtual_Services_Router_Series/index.aspx#tab=TAB_RESOURCES) for links to downloads, manuals etc. From there, you can click on [HP VSR1000 series hands-on lab](https://www.hpe.com/v2/getdocument.aspx?docname=4AA5-5811ENW). This lab guide walks through setting up a simple 3-router topology using VMware Workstation. It covers all the required commands for these areas:

* Basic management - e.g. login, SSH, DNS, NTP, SNMP, interfaces, etc.
* OSPF
* GRE
* BGP
* ACLs
* IPSec

It does not cover the theory behind those topics - you'll need to learn that somewhere else. But this is ideal if you have experience with other platforms, and need to figure out some of the HP syntax.

It's not perfect, and there's a couple of typos, but nothing major. If you don't have much experience with VMware, it might take you a bit of effort to get your networks sorted out. That's something you really need to learn anyway, so it's not wasted effort.

I'm pleased to see HP freely publishing these manuals, labs and courses. This is exactly the sort of thing you need to do when you're trying to appeal to all those network engineers with Cisco experience. Well done to whoever made it happen.
