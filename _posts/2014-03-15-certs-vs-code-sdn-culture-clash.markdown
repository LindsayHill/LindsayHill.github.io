---
author: lindsay
date: 2014-03-15 01:36:53+00:00
layout: post
slug: certs-vs-code-sdn-culture-clash
title: 'Certs vs Code: SDN Culture Clash?'
categories:
- Opinion
tags:
- certs
- SDN
---

SDN career certifications are starting to emerge. Network engineers are well-used to certifications, and use them as a badge of marking progress. But developers prefer to focus on code, not passing tests. This will be just another way that the different cultures rub up against each other as we change the way our networks work.

## SDN Certifications Emerging

HP has developed an SDN Developer certification, [HP ASE - SDN Application Developer v1](http://h10120.www1.hp.com/expertone/data_card/HP_ASE_SDN_Application_Developer_V1.html). This certification is aimed at:

> experienced application developers...with some knowledge of networking functions or networking professionals with one or more years of application development experience

Amongst other things, it validates that you can do the following:

* Understand the SDN environment including SDN application use cases, the OpenFlow standard, the HP VAN SDN Controller, and OpenFlow enabled switches
* Design an application with SDN, including decisions about application type and process of design creation; and understand examples of successful SDN-enabled applications...
* Write, test, and debug an SDN application with the HP VAN SDN Controller and OpenFlow enabled switches

Cisco is developing courses for [designing and supporting programmable networks](https://learningnetwork.cisco.com/docs/DOC-22087), and they are working on certification programs. They hype around it last year implied that they should have released this by now, but apparently they're still ironing out a few kinks:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/northlandboy">@northlandboy</a> <a href="https://twitter.com/kirkbyers">@kirkbyers</a> I know. Seem to be getting close now though. Think they&#39;re taking the time to get it as right as possible @ release</p>&mdash; Matt Saunders (@citylifematt) <a href="https://twitter.com/citylifematt/status/443899922106945536">March 13, 2014</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Cisco's certification program has been enormously successful over the last 20 years, providing a clear pathway and goals for network engineer career development. Obtaining a certification is a popular way of showing your development level to potential employers. It has become a form of shorthand to identify a network engineer as being at CCNA-, CCNP- or CCIE-level.

## Running Code is King

Developers don't go in for certification with nearly the same gusto. They'd rather show off some code, or point to an application they built.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/northlandboy">@northlandboy</a> Yuck, there are better ways to establish programming credibility than certificates (i.e. go write some open-source code).</p>&mdash; Kirk Byers (@kirkbyers) <a href="https://twitter.com/kirkbyers/status/443879299179769856">March 12, 2014</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

[GitHub](http://github.com/) has become the de-facto way of showing off your skills. If you're applying for a developer role, you can be fairly certain that they will be looking at your GitHub profile. Employers are looking at what projects you've been involved in, what sort of code you're contributing, etc. This isn't just employers either - industry peers will look at what you have published to get a sense of your abilities. Certifications are seen as a simple test that can be 'gamed' while running code stands on its own. Developers aren't interested in what test you've passed, they want to know what you've actually done.

{% include note.html content="Aside: What if your employer [doesn't let you put code on GitHub](http://www.theregister.co.uk/2014/01/22/amazon_open_source_investigation/)? How do you demonstrate value then?" %}

So what happens when developers and network engineers come together?

## Culture Clash - but we'll get over it

Changing the way we design, build and operate networks is going to involve many challenges, large and small. Someone who grew up writing code has a fundamentally different way of thinking about the network than someone who started out making patch leads. This goes beyond the network - it's about the type of person, how they solve problems, how they operate in a business environment, how they validate technical experience. There will be many other cultural clashes, and this is just a very small one that we'll get over.

I am unconvinced about certification for SDN development, but I expect to see mainstream network certifications adopt SDN topics - primarily around concepts & configuration, not development. This ties in with where I see SDN development going: only a few network engineers will do any serious development work, with a wider group doing some basic scripting + integration. Most will just drive the tools from the GUI. That's the path that sysadmins have gone down with the advent of server virtualisation.
