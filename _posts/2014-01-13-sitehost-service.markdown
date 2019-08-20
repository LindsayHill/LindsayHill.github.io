---
author: lindsay
date: 2014-01-13 08:49:10+00:00
layout: post
slug: sitehost-service
title: SiteHost - Top-Notch Service
categories:
- Opinion
tags:
- opinion
- vmware
---

We needed a hosting provider that could deliver a platform to run a Virtual Appliance on. The issue was that we needed a reasonably high level of RAM, and we really wanted something based in New Zealand. Most IaaS providers are quite expensive if you want a high memory instance, and/or something that will run 24/7. We worked out that the most cost-effective option was to lease a server, install ESXi on it, then run our virtual appliance on that.

## First Attempt - Leased Server

We found a local provider, [SiteHost](http://www.sitehost.co.nz/), that offers a range of hosting options, including website hosting, VPS, and dedicated servers. We signed up for a dedicated server, and set about installing ESXi. But we struck a compatibility issue with that specific hardware. At this point, SiteHost could have said “Tough. Not our fault.” But they didn’t.

## Second Attempt - Xen VM

They instead offered to install our Virtual Appliance (which is CentOS-based) on a Xen Hypervisor they manage. Normally they offer Windows or Linux-based VMs, not custom images. But the key is that they wanted to help, and to find a way to make it work for us.

This got the appliance up and running, and looking good…until our software support provider said “Nope, we won’t support that VM running on Xen. Has to be VMware.” Even though you and I know full well that hypervisor is a commodity these days. Back to the drawing board.

## Third Time Lucky

SiteHost stuck with us, and procured a new physical server, this time from a different supplier to what they normally use, but one we knew would work with ESXi. Eventually, this did the trick, and we were up and running.

## Learnings

You can’t always be the biggest, or the cheapest. You need to find a way to differentiate yourself. One way of doing that is by working with your customers to help them solve their problems. Amazon would have looked at the size of our account, and just said “Meh.” If our software didn’t work in their environment, tough luck for us. But SiteHost stuck with us, and persevered until we had a solution. They didn’t get hung up on what their “official” service offering was. Instead they did their best to find a way to make things work. That’s a great way to differentiate yourself, and retain customers.
