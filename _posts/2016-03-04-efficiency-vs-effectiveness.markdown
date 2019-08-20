---
author: lindsay
date: 2016-03-04 11:46:23+00:00
layout: post
slug: efficiency-vs-effectiveness
title: Efficiency vs Effectiveness
categories:
- Opinion
tags:
- SDN
---

I’ve been wondering about how we’re approaching networking change. We know we need to make things better. Are we changing the ‘right’ things? I’ve got a feeling that we’re not, but I suspect that we’re too constrained by higher-order systems.

[Simon Wardley](https://twitter.com/swardley) wrote a great post on [Efficiency vs Effectiveness](http://blog.gardeviance.org/2015/12/efficiency-vs-effectiveness-repeated.html). He gave a slightly contrived example of an organisation that is optimising the wrong thing. They plan on using robotics to automate server modifications to fit their custom racks. The problem is that they miss the point altogether. Yes, they’re optimising their flow. But they should ask: Is this the right flow?

## Cheques: Apparently people still use them?

Recently I came across the “[Wells Fargo Mobile Deposit](https://www.wellsfargo.com/mobile/apps/mobile-deposit)” application. It sounds good - a faster way to deposit cheques(checks):

> Mobile Deposit is secure, easy to use, and convenient.
>
> * Deposit checks directly into your eligible account using your Android or Apple® mobile device or your Windows Phone.
> * Take photos of the front and back of your check and submit. It’s that easy.
> * Get confirmation on your device and by email for each successful deposit.
> * Save time with fewer trips to an ATM or store.

Except…did anyone tell them that cheques are antiquated relics? That they come from a when a man’s word was his bond, and failure to uphold said word was just cause for a duel? Did anyone tell them that the rest of the world has moved on, and cheques represent a vanishingly small proportion of financial transactions?

If you’re forced to deal with cheques, then sure, this looks like a good way to make them slightly less painful. But user needs don’t start with “I need to write or deposit a cheque” No, they start with “I need to send or receive money.”

What’s a better use of time & energy? Optimising a broken & crappy process, or moving to a better process altogether?

## Networking: Automate our existing tasks? Or change model?

I’ve been wondering about this in the context of network automation. There’s a lot we can do to automate our existing processes better:

* Script ACL deployment (fewer fat finger mistakes!)
* Write a portal (with APIs) for server teams for provisioning edge ports
* Scripted pre- and post-change tests

But sometimes I wonder if we’re doing the right thing. What’s a better use of my time & energy? Figuring out how to automate my DMVPN config, sacrifice a few chickens, and finally get PfR working? Or should I be researching SD-WAN solutions?

If I’m running a smallish DC, should I bother setting up an auto-provisioning system for server ports, so we can build a private cloud? Or should I help my business move to all public cloud? Rather than optimising our current process, should I be figuring out a better process altogether? What gives the best outcome for my business?

## But…you can’t control what users ask for

In Wardley’s example, we could see & control the whole process. We could see why it was silly to optimise in the wrong place. We probably had enough control to make a change too.

But whatever process we’re working on is usually part of some larger value chain. We can’t always change the user needs that we take as input. Sometimes the higher order systems/users expect things to be done a certain way, and we can’t change that. We have to work with the inputs we have.

Networking is a classic example of this. We have to fit into a broader system, and it’s hard to change that. Too many entrenched processes, too many integration points, too much inertia. It can change, but it takes time. Look how long it’s taking to roll out IPv6.

Application developers can laugh at us, as they switch to another ‘hot’ new language more often than Thailand has coups. That’s easy when you’re operating a higher order system. But networking is lower in the stack. Changing interfaces takes time.

So I guess we’re just going to have to polish that turd some more, doing what we can to make things better in some small way. Bigger change will take longer.
