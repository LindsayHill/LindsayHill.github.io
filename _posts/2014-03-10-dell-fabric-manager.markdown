---
author: lindsay
date: 2014-03-10 01:37:26+00:00
layout: post
slug: dell-fabric-manager
title: Dell Fabric Manager & The Future for CCIEs
categories:
- Routing &amp; Switching
tags:
- NFD
---

[Network Field Day 7](http://techfieldday.com/event/nfd7/) had many interesting presentations and discussions. I'm still working through them all, but one from [Dell](http://www.dell.com/) caught my eye, where they declared that CCIEs were no longer required. They were slightly exaggerating their case for impact, but there is some substance to it - we shouldn't need CCIEs to generate basic network configurations for standard networks.


## Bearer of Bad News?


It's a brave person that walks into a room full of CCIEs and Consultants, and says something like:

> I hate to be the bearer of bad news, but the CCIE certification, the days of consultants...they're limited in my mind.


Particularly when those CCIEs are also [Network Field Day](http://techfieldday.com/event/nfd7/) attendees, and they have no hesitation in telling the speaker exactly what they think. But that's what Arpit Joshipura said during the [Dell presentation at NFD7](http://techfieldday.com/appearance/dell-networking-presents-at-networking-field-day-7/) (around 17:40):

<iframe width="560" height="315" src="https://www.youtube.com/embed/IwhFg2PMaFA?ecver=1" frameborder="0" allowfullscreen></iframe>

This was part of a presentation on Dell's [Fabric Manager tool](http://www.dell.com/nz/business/p/dell-fabric-manager/pd), which can design your network, generate a Bill of Materials, create a wiring diagram, export a network diagram to Visio, generate the configurations, and deploy them to bare metal switches. Oh and tell you if you'd plugged the cables in the wrong way. It was unclear if it also made coffee, but maybe that's in the next release.



## Pain Points & Time Wasters



Manual creation of basic configuration files is a huge waste of time. Most experienced network engineers have developed their own templates they use, and some companies have developed their own internal configuration generation/management tools. But it's such a waste of time for us to all be doing it ourselves. There's no unique value there to an organisation. It just wastes time - either internal engineers, or external consultants.

In the last 12 months I've come across several organisations that were having trouble getting "reference" configurations for their new [HP Networking](http://hp.com/go/networking) gear. It's easy enough to get basic connectivity working, but we all know that there are many functionality and security enhancements that can be made on top of a basic switching configuration. It was taking far too long to get validated, tested configurations. In one case, a missed command caused hours of wasted time tracking down a problem.

It was all so unnecessary really. Everyone likes to think their network is a precious snowflake, but the reality is that most networks are pretty similar. Cisco's [Validated Design Program](http://www.cisco.com/c/en/us/solutions/enterprise/validated-design-program/index.html) certainly goes a long way towards creating standardised network, but this seems to take it further, with full configuration generation. I'd like to see tools like Dell's available for all major networking vendors.



## Future for CCIEs?



Does this mean CCIEs are no longer required? No, I don't think so - but their role should continue to shift. If your value as a CCIE is in banging out configurations, then yes, that is in decline. There is still a requirement to be able to identify key features of your design, drive a Fabric Manager-type tool to create the configuration, and be able to do troubleshooting when it breaks. It **should** mean that network implementations can be done faster, therefore reducing the length of consulting engagements. That could mean that there is less work for CCIEs - but it could also mean that more time will be available to helping customers actually do **interesting** things with their network. Ultimately that's what it's all about - delivering services via the network. No-one really cares about the underlying configuration files.

**UPDATE:** The NFD7 delegates discuss this a bit further in "[Packet Pushers show 182: The Future of Networking Part 1](http://packetpushers.net/show-182-the-future-of-networking-part-1-as-inspired-by-nfd7/)" - but you're already subscribed to the Packet Pushers, right? Right?
