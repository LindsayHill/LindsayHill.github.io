---
author: lindsay
date: 2015-04-28 20:00:33+00:00
layout: post
slug: musing-generalist-to-specialist-and-back-again
title: 'Musing: Generalist to Specialist and Back Again'
categories:
- Worklife
tags:
- worklife
---

Recently I’ve been musing on IT Generalists vs Specialists. We used to have more generalist roles, covering all parts of the stack. ITIL then pushed us towards greater specialisation. I believe that we’ve gone back to valuing the Generalist, as the person who can glue components together. Will the pendulum swing again?

## Generalists: Soup-to-Nuts

When I started working in IT, our roles were more generalist in nature. We did everything. To set up a new app, we racked the servers and switches, installed the OS, configured the network, installed the DB & application, and made it all work.

We weren’t specialists in any one area, but we knew how everything fitted together. So if something broke, we could probably figure it out. If we had to investigate a problem, we could follow it through all layers of the stack. When we found the problem, we had license to fix it.

## ITIL takes over: Specialisation

Sometime around the early-mid 2000s the “ITIL Consultants” moved in. Their talk of structure, processes and SLAs seduced senior management. We couldn’t just have people who Got Shit Done. No, everyone needed to be placed in a box, with formal definitions around what they could & couldn’t do.

System scale was growing rapidly at the time, but automation tools & management were lacking. Specialisation seemed to make sense - if you have a narrow list of tasks, you should be able to do them faster and more reliably.

The problem was that people became responsible for their individual components, not the service. It was too easy to pass the buck. “No, the network’s fine, it must be the application.” “No the application’s fine, it must be the database…etc.” Teams hid behind process, shuffling tickets between queues. “Service Delivery Managers” reported on queue lengths, SLA breaches and ticket resolution times.

Meanwhile, things were still broken for end-users…and somehow no one team was responsible.

## Generalists Strike Back

Things have changed over the last few years:

* COTS components & systems are often ‘Good Enough.’
* SaaS is practical for many common applications.
* Sites like Google + [StackExchange](http://stackexchange.com) give everyone more than enough information to be dangerous.
* Virtualisation means that admins need a better understanding of how compute, networking and storage work together.
* Automation and management frameworks have come a long way.

Combine this with an appreciation that enforced silos resulted in worse service outcomes. I believe that is why we’ve seen a rise in roles like Site Reliability Engineers, and more recently cultural shifts to models like DevOps.

I believe that this means there is a greater need for the IT Generalist. Deep knowledge of individual pieces is less important than knowing how the pieces fit together. That arcane knowledge on how to tune Sendmail for better performance doesn’t matter in a world where compute resource is plentiful. But understanding what those network options mean in vCenter, and how you should deploy that vApp to integrate well with the existing systems? That is useful.

Is there still a place for specialisation? Of course. But fewer organisations need them today.

Will the pendulum swing back towards specialisation in future? Maybe. We do like to move in cycles. What do you think?
