---
author: lindsay
date: 2016-03-15 04:22:50+00:00
layout: post
slug: networkings-not-so-bad
title: Networking's not so bad
categories:
- Opinion
tags:
- rant
---

[Ivan’s post](http://blog.ipspace.net/2016/03/speaking-of-cli.html) this week was a good reminder that other parts of IT aren't perfect either. It’s not all roses on the other side of the fence. Networking has done many good things, and often showed the way.

Consider a conversation between a sysadmin & a network engineer:


> Look at how I can virtualise these systems! Now I can isolate users and consolidate hardware resources. They have no idea they’re on the same hardware. It's incredible!


Oh. Bit like these VLANs, VRFs, and VDCs we’ve been doing for 15+ years now?


> Look at how I can use Puppet to define this server’s complete configuration using a single text file! This is amazing! I can use version control for my infrastructure!


Oh. You mean like this single text file that defines the configuration of my network device here? Yes, yes that does seem useful.


> Why do you networking people have so many different ways of configuring systems? Why don’t you just have one common API?


Oh. You mean like the way that there’s a [Universal install script](http://xkcd.com/1654/) Linux systems?


> SNMP sucks. The data format is terrible, implementations are inconsistent. Why don’t you switch to gRPC?


Wait, weren’t you telling me last month to use JSON? No wait, it was XML before that. But obviously it’s _hopelessly_ outdated now. Should be using SOAP? No, that’s right, we’ve moved to REST. Ah no, CIM is the data model that’s going to rule us all! Wait, when does that one arrive? I lose track.

Networking’s not perfect. Don’t get me wrong. But my point is: Don’t get down about networking. Be wary of assuming the grass is always greener on the other side. Those network engineers of the past have achieved amazing things. We’ll do amazing things in the future. We just need to keep evolving.
