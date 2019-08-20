---
author: lindsay
date: 2014-11-15 20:00:38+00:00
layout: post
slug: ops-work-vs-project-work
title: Ops Work vs Project Work
categories:
- Worklife
tags:
- operations
---

There's a constant tension between delivering new services, and running the existing services well. How do you figure out how to prioritise work between Operations tasks and Project work? Skewing too far either way leads to problems. Maybe the answer is in how we structure Operations tasks?

## Definitions

* **Operations work:** Dealing with outages, trouble tickets, support requests, etc. System monitoring - reviewing data for capacity planning, and identifying new areas to monitor. Automated repetitive tasks. Patches, upgrades, minor changes to existing services. Accountants would call this work [OpEx]({% post_url 2013-09-06-accounting-and-cloud %}).
* **Project work:** Design, test and deployment of new services. Major upgrades or enhancements to existing services. This is usually classified as [CapEx]({% post_url 2013-09-06-accounting-and-cloud %}). For some businesses, this work is customer-billable.

## What happens when you’re imbalanced?

* **Too much Project work:** If you’re flat out deploying new systems (and dealing with the fallout), it’s easy to let Operations work slip. Maybe you don’t get around to automating that log rotation script, or paying attention to the slope of that consumption graph. It’s OK for a while too…things seem to be trucking along. But then you start having outages due to simple things like logs filling directories, or you hit a capacity limit, and there’s a 6-week lead time on new hardware. Once it gets really out of hand, you’re running around like mad fixing broken systems. You’re too busy with firefighting to do either Project work or real Operations work. Not a good place to be in.
* **Too much Operations work:** Sometimes Sysadmins think that everything would be fine if they could just be left alone to run the current systems. No more constant churn of upgrades and new applications. But businesses don’t stand still. You would quickly stagnate. Make it too hard to introduce new systems, and you’ll end up with people going around you - e.g. Shadow IT, skunkworks projects, and outsourcing. You don’t want to end up like “[Mordac, the Preventer of Information Services](http://search.dilbert.com/comic/Mordac%20The%20Preventer).”

## One Approach: Projects first

(Aha, but what defines a project?)

So how do you balance Operations vs Project work? [The Practice of Cloud System Administration](http://the-cloud-book.com/) has this to say:

> To assure that all sources and categories of work receive attention, we recommend this simple organizing principle: people should always be working on projects, with exceptions made to assure that emergency issues receive immediate attention and non-project customer requests are triaged and worked in a timely manner.

_(Section 7.3)_

They go on to recommend a rotating shift where one person is on-call for emergency issues, one is handling regular tickets, and everyone else works on projects.

The key though is that their definition of projects includes a focus on tasks that “automate or optimise aspects of the team’s responsibilities.” These are things that a good Operations team does, but by structuring them as a project, they can be prioritised, tracked, and have resources allocated.

This is a big step up on how most Operations team work, where they don’t structure their work like that, and it gets no focus. It gets lost under the higher-prioritised projects.

## How do you prioritise an ‘Operations’ project?

It’s not enough to just structure your Operational improvement work as projects. All projects (capital and operational) need to be prioritised. Many businesses will focus on those projects that they think deliver an immediate return - and that’s usually the capital projects. You still need to figure out how to make sure that you get the people and money allocated to Operational improvement works.

Limoncelli et al recommend monitoring how much time is being spent on ‘toil’, or manual tasks:

> If more than half of a team’s collective time is spent on operational “toil,” that fact should raise a proverbial red flag. The team should review how the members are spending their time. Usually there are a few deeper “root cause” problems that could be fixed to eliminate large chunks of manual work. Establish projects that will strike at the root of those problems. Put all other projects on hold until the 50/50 balance is achieved again.

_[The Practice of Cloud System Administration](http://the-cloud-book.com/), section 12.4.2_

You might play around with the exact split there, but I like the idea of monitoring toil, and if it gets too much, make sure something gets done about it. But it also means that you will have some level of 'toil', and can't totally avoid new service delivery. This method still needs management support, as sometimes you’ll have to say “Sorry, we can’t deploy that new service right now: We have to improve some existing systems first.”

I’m not sure that there is any one right answer here. Clearly organisational type and structure plays a part in it. How do you divide up your work, and ensure both streams get the attention they need?
