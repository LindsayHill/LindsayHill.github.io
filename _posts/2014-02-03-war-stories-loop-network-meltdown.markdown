---
author: lindsay
date: 2014-02-03 18:00:21+00:00
layout: post
slug: war-stories-loop-network-meltdown
title: 'War Stories: Loops that Permanently Broke the Network'
categories:
- Routing &amp; Switching
series: 'War Stories'
tags:
- war stories
---

{% include series.html %}

Inspired by the Packetpushers [War Stories](http://packetpushers.net/show-173-war-stories-from-the-hot-aisle-the-nightmare-before-christmas-part-1/) episodes, I thought I'd start an irregular series sharing a few of my own war stories. Hopefully the statute of limitations has expired. First up, the time that looping cables didn't just cause a network melt-down, it actually caused physical damage.

## L2 Loops are Bad, OK?

We've all heard stories about the bad things that can happen with you get a L2 loop in your network. Your switches start lighting up like a casino, and effectively all traffic stops, often including control-plane access to the switch.

<iframe width="560" height="315" src="https://www.youtube.com/embed/g7-FADwzycY?ecver=1" frameborder="0" allowfullscreen></iframe>

Remove the loop, and everything returns to normal. You go out and track down what caused the loop, and shake your head in wonder at how someone could take a cable coming out of the wall, connect a dumb hub, and then connect from the hub back to the wall. Were they trying to double their bandwidth? Hopefully you put in place some protection mechanisms so it doesn't cause a loop next time they do it. All good.

## Bad, but bad enough to break the switch?

We had several stacks of desktop 1Gb PoE switches, all working pretty well. After a few months we started getting reports that some switchports weren't working. It was just a few ports, on one particular stack. The failures came over a few weeks, and it took a while to notice the pattern. Swapping the cable over to an unused switchport worked, proving it wasn't a cable fault. We added a comment to the switchport to say it should not be used. It wasn't a case of being error-disabled, or similar - even a full switch reload didn't help, the switchport just would not come up.

Eventually I realised what was happening. All the failing ports had been mapped to a specific meeting room, and that room had a Polycom speakerphone. This was in the early days of PoE, and things had only recently been standardised. At the time Polycom didn't have PoE-compliant systems. Since they only wanted to have one cable running to the speakerphone on the table, they had their own Power Injector. You ran a power supply and Ethernet cable to the Power Injector, and then a single Ethernet cable ran from there to the speakerphone itself. This delivered the comms and power that the base unit needed.

The problem was that whatever they were doing not only wasn't PoE-compliant, it was also had the potential to fry switchports. It turned out that meeting room users were unplugging the Polycom, then somehow reconnecting the "hot" end back to the wall ports. This was frying our switchports, permanently disabling them. It didn't create a Layer-2 loop - it completely broke that switchport!

## Laptops too

On a related note, we had one user that was onto his fourth laptop - for some reason the network ports kept failing on his laptops. They would either stop completely, or maybe only connect at 100Mb, and refuse to connect at 1Gb. We realised that this person was a regular user of that meeting room - he'd obviously been regularly unplugging the Polycom unit, and connecting his laptop. The laptop NIC didn't like the power injector either, and poof! Out came the magic smoke.

## Mitigations

What can you do to stop it? Polycom had shipped a cable that had a very short retainer tab, making it difficult to unplug the Polycom end. People were determined though. The best we could do was to attach a big label to the cable, threatening people on pain of death if they unplugged that cable and used it for their laptop.

These days you can just use regular PoE, with no need for a power injector. If you loop a PoE cable back to a switchport, it won't break it.
