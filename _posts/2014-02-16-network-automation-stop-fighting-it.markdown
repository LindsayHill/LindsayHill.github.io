---
author: lindsay
date: 2014-02-16 03:19:49+00:00
layout: post
slug: network-automation-stop-fighting-it
title: Network Automation - Stop Fighting It
categories:
- Opinion
tags:
- SDN
---

Network Engineers should be embracing the idea of automating away the drudgery of running a network. They should be looking for ways to ensure the network can dynamically change its operation to ensure optimal application performance. But it seems there will be a lot of resistance to this, and it won't be technical: It will be cultural.

## Polling Public Opinion

Last year [Ethan Banks](http://ethancbanks.com) put up a post on [Thwack](http://thwack.solarwinds.com/) [asking](http://thwack.solarwinds.com/message/220637):

> Would you automate QoS policy based on real-time flow data?"

You can argue about the very specific use-case here, but it could be considered as a more general question: “Would you trust automation to dynamically change your network in response to changing conditions?”

There was quite a bit of activity, but overall I found the responses quite unsettling. Here’s some typical answers:

> ”Sounds like a pain to me. As I have said before, I would rather make changes manually…” [wluther](http://thwack.solarwinds.com/message/220647)
>
> ”sounds like a nightmare to support. Advice is a great idea, but don’t automate changing the stuff I have to support” [supermon](http://thwack.solarwinds.com/message/220627#220627)
>
> ”Dynamic management can be dangerous and unpredictable.” [storm](http://thwack.solarwinds.com/message/220677)
>  
> ”Simply no. Your network configuration should be very intentional. Having it change automatically is asking for trouble” [donpepe](http://thwack.solarwinds.com/message/220765)

…And so on. It depressed me.

## Why are we obsessed with doing a computer’s work for it?

That post was gnawing away in my mind for a while, until I saw a link to this [Usenix presentation](https://www.usenix.org/conference/lisa13/technical-sessions/plenary/underwood), that contained these quotes:

> Let us stop doing the machines’ work for them.
> Let us stop feeding the machines with human blood.

That struck at the heart of what was troubling me - too many of the responders were still wanting to do a computer’s job. It is an understandable response to seeing the threat of their current day to day job disappearing, but I think they are missing the point of where they add value. Hint: It’s not in twitching your fingers at the CLI, it’s about figuring out what the business needs, and using technology as appropriate to help them achieve it.

## You’re already using automation - and so is everyone else

So there’s some resistance there to using automation to dynamically manipulate traffic flows on our network. But let’s consider a few points:

* Dynamic Routing: You’re almost certainly using a dynamic routing protocol that adapts to changing conditions. If I asked a network engineer to configure a network entirely using static routes, they would look at me like I was crazy. “But how will it adapt to a router going down? I can’t go and manually change routes every time a link flaps!!!” Exactly. It seems that people are OK with automation for dealing with faults, but haven’t yet realised that performance issues are a kind of fault, and we should be able to adapt our networks to cope.
* DRS: A typical VMware cluster will be dynamically moving loads around, based on CPU and RAM usage. It may even be moving datastores around based upon I/O. The current weakness in DRS is that it doesn’t move loads in response to network conditions - but there’s no reason it couldn’t/shouldn’t. If it’s OK for sysadmins to use dynamic load management, what makes the network so special that it can’t be touched?
* Automated flow control is already happening with ISPs. When my ISP detects certain types of DDOS traffic, they have automated response systems that kick in & black-hole the traffic. Humans can’t respond quickly enough in these situations: Machines can.

## Change Management Concerns?

I also found this quote from [mikegrocket](http://thwack.solarwinds.com/message/221060) interesting:

> "An interesting concept and I like it, but this is a non-starter in my world. Too much CM to get through to make changes. Nothing is changed in-real time…Govt life, gotta love it.”

I understand where he’s coming from with that, but here’s a funny thing: ITIL makes it very difficult for you to make frequent changes to your network - but no-one questions an automated process. Change prevention^Wmanagement teams don’t like **you** going and making regular changes. But if you implement an automated process, it can take whatever actions it thinks are needed, within its defined parameters. Consider a cron-tab - do you manually login and run scheduled jobs? No you do not, and no change management process has an issue with scheduled jobs. Does ITIL stop you using OSPF? Nope. Mind you, maybe no-one’s explained DRS to the change management teams, so they haven’t had a chance to veto it?

## A Glimpse of the Future?

The [VMware Network Virtualization Blog](https://blogs.vmware.com/networkvirtualization) had a recent post on [“Elephant Flow Mitigation”](https://blogs.vmware.com/networkvirtualization/2014/02/elephant-flow-mitigation.html). This described a solution that HP and VMware are working on, where the Open vSwitch in a hypervisor can detect elephant flows, and signal this to the SDN network controller, which can then instruct the physical network to apply specific handling to those flows.

A related SDN application from HP can communicate with Microsoft Lync, to help with detecting Lync application flows, and [dynamically configure QoS](http://www.nojitter.com/post/240153039/hp-and-microsoft-demo-openflowlync-applicationsoptimized-network) policies to ensure a good user experience.

These are good examples of how applications can interact with the network, to deliver a better overall experience. Humans simply can’t react that quickly. Sure, we should be setting policies and bounds - e.g. Setting priorities between different applications requesting resources - but it’s up to the machines to implement those policies.

We need to let go of past prejudice, and recognise what’s important about what we do, and what could be better done by an automated system. A closing thought from [Agent Smith](http://www.imdb.com/title/tt0133093/quotes):

> Never send a human to do a machine’s job

[![Agent Smith](/assets/2013/09/agent-smith.jpg)](/assets/2013/09/agent-smith.jpg)
