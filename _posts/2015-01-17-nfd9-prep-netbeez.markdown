---
author: lindsay
date: 2015-01-17 02:55:00+00:00
layout: post
slug: nfd9-prep-netbeez
title: 'NFD9 Prep: NetBeez'
categories:
- NMS
tags:
- NetBeez
- NFD
---

I'm reviewing the presenters for [Network Field Day 9](http://techfieldday.com/events/nfd9), in particular looking at those I'm not familiar with. [NetBeez](http://netbeez.net/) is one of those making their first Tech Field Day appearance.

## NetBeez

We all know that our users and the applications they access are incredibly distributed. We don't control all the network elements, but the network team still gets the blame if things go wrong. You need greater visibility to prove it's not the network, but getting that visibility is tough. Current options for probes aren't always cost-effective to deploy across many sites. Many sites don't have any local server infrastructure.

That's where NetBeez comes in. They have developed [Raspberry Pi](http://www.raspberrypi.org)-based agents that can easily be deployed to many locations. Plug in power, plug in a network cable, and it phones home. Go to the NetBeez dashboard, and from there you can configure the tests you want the agent to run.

Since the devices are so small, they can easily be deployed to a range of small sites, and can simulate a range of user traffic. Tests include Ping, HTTP, Traceroute, DNS. A particularly nice feature is the ability to run an ad-hoc [iPerf](https://iperf.fr) test with custom parameters.

The dashboard shows you how the tests are performing, across the different locations they're running from. Now you can see if the problem is just affecting retail stores on DSL circuits, or if it's all users.

While you could build your own agents to do all this testing, that's a hassle that most of us don't want to deal with. NetBeez handles all the agent management - software updates, configuration, etc.

I'm also pleased to see that they are **not** based in Silicon Valley.

> ...we are located in Pittsburgh, and Pittsburgh rocks, simple as that.

[NetBeez Careers](http://netbeez.net/careers/)

Not that I've got anything against Silicon Valley - I think I might like to work there. It's just that it's great to hear about interesting firms based in other parts of the world.

## Troubleshooting Challenge

NetBeez is running a [Troubleshooting Challenge](http://netbeez.net/the-great-troubleshooting-challenge/?utm_source=NetBeez&utm_medium=LearnMoreTab&utm_content=HaveWhatItTakes?&utm_campaign=Troubleshooting%20Challenge), starting January 20th at 12:00 EST.

> _Participants will receive access to our Dashboard and instructions once the contest begins.  Once logged in, users must troubleshoot a network or application problem that is affecting the end user._

This sounds like a great way to raise company awareness. Nothing shows off a product better than getting your hands onto it and solving a 'real' problem.

Maybe I shouldn't be telling my readers about it though, to try to improve my chances??

## What I'm Looking For

These are some of the topics I'm interested in:

* Deployment models for the server-side - cloud-based vs on-premises.
* Other agents - e.g. virtual agents, or installable packages
* Pricing models - how does the pricing scale?
* Data extraction + integration with other monitoring systems, etc.
* Options for running other application tests

I'm looking forward to seeing what they have to say.
