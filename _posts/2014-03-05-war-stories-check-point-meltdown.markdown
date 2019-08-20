---
author: lindsay
date: 2014-03-05 21:15:56+00:00
layout: post
slug: war-stories-check-point-meltdown
title: 'War Stories: Check Point Meltdown'
categories:
- Security
series: 'War Stories'
tags:
- Check Point
- war stories
---

{% include series.html %}

Firewalls are usually deployed as a cluster, to provide failover capabilities. Protocols such as VRRP are used to that traffic is normally routed via one node, but if that node fails, the other one automatically takes all traffic. Connection synchronisation ensures that the backup firewall is always aware of all active sessions, so that a failover will be almost seamless.

This works pretty well - I spent years looking after a lot of Check Point clusters, predominantly on Nokia IPSO, later on SecurePlatform. Don't hate me for it. Occasionally we'd have a power supply or disk drive failure, and traffic would seamlessly switch over to the standby node. It also worked well during upgrades - demote the node to secondary, patch it, promote to primary, test, switch back if needed.

## It's not true redundancy

All good. But there's a problem - you don't have TRUE redundancy. You're protected against hardware failure, and you've also got some mitigation strategies to deal with problems related to software upgrades. But what happens if you have a software problem? Normally you're running exactly the same code and configuration, so if a problem affects one node, it might affect both nodes. Check Point configuration is managed on a per-cluster basis too - when making firewall policy changes, you push the policy to both firewalls at once. You don't do them in sequence.

That's what happened to me - we had a software bug that took out the whole cluster.

{% include note.html content="Note: This story is about a Check Point cluster, but it's not a specific attack on Check Point - this is a general problem that applies to any system that has shared code/configuration across a cluster" %}

## Meltdown

Check Point used to have a feature called "Smart Defense". These days it's known as IPS. It's an integrated intrusion detection/prevention system within the firewall. It's had a chequered history, with engineers heard to mutter "If it makes no sense, it must be Smart Defense." Certainly I've seen some odd behaviour in the past, where it would drop specific sessions without bothering to log any indication of why it dropped them, or even admitting that it had dropped the session.

Check Point releases signature updates for SmartDefense/IPS on a frequent basis. Open up the Policy Editor, and it will say "Hey, these new signatures are available. Do you want to download/enable them?" One morning I saw a new signature for detecting specific types of malicious OSPF traffic. That sounds interesting. Maybe I'll just put it in "Detect" mode, so it will tell me if it sees anything, but it won't drop it. Can't hurt right? It's just detecting it, not dropping it.

Tick the box, push policy, done. Make note to check logs later to see if any of that traffic has been detected.

A few minutes later...complete meltdown. Everything's offline. Nothing reachable in the data center. Can't get from Enterprise network to DC, and it seems that all intra-DC traffic is broken. What the hell's going on? After a bit of scrambling, we realise that the firewalls seem to be blocking everything, via all the virtual firewalls we have on that VSX cluster.

Can't remotely login in to them, so we wheel out the [Crash Cart](http://en.wikipedia.org/wiki/Crash_cart#In_computing), and plug a monitor into the firewall. Kernel Panic. Oops. That's not good. Power-cycle the firewall, and it comes back up, and starts passing traffic for a minute or two...then kernel panic again. Oh no....more testing shows that if we reboot both firewalls, one goes master, takes a bit of traffic, then kernel panics...so the secondary takes over, passes traffic for a minute...and also kernel panics. Redundancy didn't help here.

What can we do? Check Point firewalls are managed by a remote management server. You can't change the configuration locally, like with an ASA. You MUST make changes from the management server...but how can I push out an updated policy, if the firewall won't stay up long enough to install policy?

Oh and did I mention that the company had a major launch about to happen, within an hour? Yeah, you could say that there was a certain amount of panic, and people lighting their hair on fire and running around.

I knew that I needed to get back to the configuration I had installed yesterday. How to do it? I had backups for all systems - the management server & the firewalls. With Check Point systems, the backup from the firewall doesn't really matter much anyway - you can just re-build the firewall, and push out policy again. So I ended up doing a restore on the management server to yesterday's configuration, rebuilding the firewall from scratch, and pushing a working policy out to it. Service working again! All done within an hour too, I might add.

## Lessons Learned

1. Have backups, know where they are, and how to restore from them. I'd had enough system failures over the years to know I needed to have that scripted up. You'd be surprised how many people don't do have backups of network devices.
2. Have a good team around you, and trust them. I was lucky that I had a good group of people around me, and we'd spent years doing Operations work together. We knew how to handle those sorts of situations. One person ran interference, and cleared out space around the people working on the problem. This got rid of all the noise, and gave us time and space to figure out what had happened, and how we were going to recover. The worst thing you can do is stand over the engineer's shoulder, and ask "Is it fixed yet?" every 5 minutes.
3. Redundancy is more than just hardware. Understand what the failure domains are. This is partly why I don't like stretched L2 designs - you're providing some redundancy against hardware failure, but you still have a shared component that could take out everything. Ideal scenario would have been to have another data center that we could run all services out of, but at the time we didn't have the budget.

It was a rather "interesting" morning all around, but we were able to recover, and life goes on. That's the good thing about networking - once you've fixed the problem, stuff starts flowing again. It's a lot more entertaining dealing with outages that involve lost data. Sometimes that stuff can never come back.
