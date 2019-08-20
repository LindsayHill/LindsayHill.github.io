---
author: lindsay
date: 2013-08-21 04:33:36+00:00
layout: post
slug: monitor-service-provider
title: Should I Monitor My Service Provider?
categories:
- NMS
tags:
- monitoring
- outsourcing
---

How much monitoring should I do for services that I’ve outsourced? This question comes up frequently with my clients. They’ve paid someone else to manage a service for them, and they’re wondering if they should just leave all monitoring to the service provider. They’re paying for it, so why have a dog and bark yourself? This applies to all sorts of outsourcing agreements - it might be for a simple SaaS-based service, or it might be a complex network environment that you designed and built, and then handed over to a third party to manage.

## I paid for it, didn’t I?

The general thinking is that you’ve outsourced the service, and you have contractual agreements for the provider to perform specified monitoring, with SLAs, escalation procedures, and all that tedious stuff in place. So if the outsourcer is doing all that, and I’m paying an eye-watering amount for it, why should I need to do any monitoring myself? Isn’t that all covered? Don’t they respond to faults, and provide me monthly reports? Yes, but…

## Trust, but verify

Mostly the provider does things well, and you have a certain amount of trust in them. But trust alone isn’t enough. Things can change - people come and go, the new platform gets oversubscribed, etc. You still need some verification that things are working, at least at a basic level. If you’re a typical organisation, you will have multiple applications and services provided by a variety of external organisations. But you’re still running the internal Service Desk, which is the first port of call for your users. You want to get some early notification if there are issues. Looking at the (usually outdated) “network status page” doesn’t help. You want to get some basic notification in your own alerting tools if there’s any major problems.

The other thing to keep in mind is that if you’ve purchased some standard service at $50/year…well that gives you a pretty good idea of just how much the provider cares. Even SLAs might not help you - a provider can decide it’s more cost-effective to breach their SLAs, rather than resolve the issue in time. They might calculate that it costs $x, and they can sustain it, but for your business it could be catastrophic. You need some ammunition to know how they’re performing, so if you need to change providers, it’s an informed business decision.

By performing basic user simulation testing, you can get an idea of A) Is the service up and responding, and B) Has the performance changed significantly? The only way you can truly rely on this data is if you’re collecting it yourself. This doesn’t have to be done from your own systems - e.g. there’s plenty of cloud-based web monitoring solutions you can use, at a fairly low cost.

## Don’t go overboard

All that said, don’t go overboard. Don’t expect to monitor every component of the provided service - you shouldn’t care about it. I’ve seen people want to get deep infrastructure monitoring for a SaaS service they’ve purchased. Don’t waste your time. It’s not up to the provider to give you that level of access, and nor should they have to. You’re not paying for a specific CPU utilisation maximum - you’re paying for a working service. How they do it is up to them. Your only real concern is the end-user experience.

Summary: Don’t try and second-guess everything your provider is doing. There’s a reason you paid them to look after this, right? But don’t completely absolve yourself of responsibility - you need to keep an eye on them to make sure they’re still delivering for you. At the least, you’ll know if things are seriously broken, before the provider gets around to telling you.
