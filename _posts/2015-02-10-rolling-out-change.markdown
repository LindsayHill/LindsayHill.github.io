---
author: lindsay
date: 2015-02-10 19:31:11+00:00
layout: post
slug: rolling-out-change
title: Rolling out Change
categories:
- Worklife
tags:
- worklife
---

We all know that "Change is Hard." But often we, as engineers, focus on the technical aspects of that change. How do I minimise customer impact while upgrading those routers? How can I migrate customer data safely to the new system? But we can forget about the wider implications of what we're doing. If we do, we may struggle to get our changes implemented, or see poor take-up of new systems.

## Why Can’t I Make That Change?

I was talking to an engineer who had planned a huge configuration management implementation. Everything had been manually configured in the past, but this was hitting scale issues. So he had worked for months on a fully automated process. It was going to be amazing. It would configure everything, across all systems and applications. Standards enforced, apps deployments done in a repeatable way, etc. It was going to be a thing of beauty. No-one would ever need to login to a server again. Total automation.

It was all tested, and was just waiting for approval to put it into production. But for some reason, no-one was willing to give the go-ahead to roll it out. Weeks were dragging by, and things were going nowhere. Inefficient manual processes continued.

Why? On the face of it, the system was going to deliver massive benefit. It would reduce re-work, speed time to deployment, and reduce outages. Why wouldn’t they give agreement to go ahead?

It was because the engineer hadn’t thought through the wider implications. He'd focused on the end-state, and not on what it would take to get there. This is a common mistake.

Here’s some of the life learnings I’ve picked up on implementing change:

## Build up Trust

Trust is important. You need to build up trust that your change is going to do the right thing. In the configuration management example, don’t try and do everything in the one hit. Build up trust, by rolling out configuration management in stages. Focus on simple things, like enforcing standard OS settings. Build up towards orchestrated application deployment.

On the flip side, if you’ve got a good record, people will let you do more at once. They trust that you’ll do the right thing. Don’t abuse it.

## Capacity for change

[Willpower is a finite resource](http://io9.com/your-willpower-is-finite-513737154). You can only exercise it so much, then you give in and have that 3rd beer at lunchtime.

Absorbing change is the same. You can only take on a certain amount at once. So be aware of the timing of other major projects. You might be ready to make your change, but your users might not be ready to take it on.

## Step-change, or gradual transition? It depends

When I was a boy, I often had grazed knees and stubbed toes. That meant sticking plasters, which also meant they had to come off. Do you pull it off slowly, or do you just suck it up, and ask your Dad to rip it off fast, and get it over?

Change can be the same. Sometimes it’s better to slowly introduce change. But that can mean drawing it out. It can mean an ugly transition period, when no-one knows what’s going on. But sometimes doing it all in one hit can hurt. A lot.

## Be careful when soliciting opinions

[Bikeshedding](https://en.wikipedia.org/wiki/Parkinson%27s_law_of_triviality) is real. Be careful about asking EVERYONE to give you requirements for a new system. If it’s something that people touch on a daily basis, everyone has an opinion. You have to figure out who to listen to, and what your scope is.

## Day-to-day stuff matters. A lot.

You’re planning a new architecture. You’re doing it for improved performance, better scaling and the right cost curve.

Great, but what if that tunnelling technique means that traceroute doesn't work anymore? Will that break standard Operations procedures? If you’re going to change something that people do every day, you have to get them on side. Help them understand why your making these changes, and how their workflows will change.

I’m still working on getting this stuff right. I tend to forget that just because something’s obvious to me, it might not be obvious to everyone else. But as I go on in my career, I become more aware that it’s not really about the technology. It’s about the people.
