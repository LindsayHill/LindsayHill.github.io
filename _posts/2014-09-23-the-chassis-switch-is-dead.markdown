---
author: lindsay
date: 2014-09-23 21:44:25+00:00
layout: post
slug: the-chassis-switch-is-dead
title: The Chassis Switch is Dead
categories:
- Routing &amp; Switching
tags:
- design
- SDN
---

The Chassis Switch is Dead. For most networks, chassis-based switches are no longer appropriate due to cost, inflexibility and risk. I see this as similar to servers, in that server blade chassis are no longer appropriate for most organisations. The alternatives are already better for cost & flexibility. The real question is what our management model will look like for those alternatives.

> Dead Collector: 'Ere, he says he's not dead.
> Leaf-Spine: Yes he is.
> Chassis: I'm not.
> Dead Collector: He isn't.
> Leaf-Spine: Well, he will be soon, he's very ill.
> Chassis: I'm getting better.
> Leaf-Spine: No you're not, you'll be stone dead in a moment.

(With apologies to [Monty Python](http://en.wikipedia.org/wiki/Monty_Python_and_the_Holy_Grail))

## Blade Servers…

In the late 1990s, and early 2000s, server buying patterns changed significantly. Previously we had a few “Big Iron” Unix systems, but cheaper Intel-based systems changed the economics dramatically. This lead to a rapid sprawl in the number of physical servers.

In the second half of the 2000s, server blades appeared as a seductive answer. They promised simpler management of pools of systems, greater density, better efficiencies, and operational cost savings. Vendors promised long term “investment protection”, assuring us that we could keep the chassis, and upgrade blades and I/O modules over the years. They told us that it didn’t matter if we didn’t know what our growth patterns would look like, blades would provide easy scaling.

Sounds good, right?

It didn’t work out that way. Server virtualisation dramatically changed the way we use servers. We needed high-density servers, and fewer of them. The blade server form factor didn’t work well for that.

So now many businesses have half-filled chassis, and they will never fill them. They’ll periodically put higher-capacity blades in (partly because hypervisor licensing pushed us that way). The cost of deploying high-memory blades has been relatively higher than in 1U or 2U servers…and customers can only buy blades from one vendor. Just how did the negotiation go with the vendor when you wanted to buy the second round of blades? You end up needing to provide a lot of space, power and cooling, for something that turned out to be expensive to operate and upgrade.

Some customers found that the investment protection wasn’t all it was supposed to be, as newer high-power blades required new chassis.

Unless you’re buying many racks of servers at a time, blade chassis are not the right answer. 1U or 2U servers are a much better choice for providing scale and flexibility.

## What about Switch Chassis Then?

I believe the same applies to switches. For many years, we used large chassis-based architectures in part due to network design constraints. See some of @etherealmind’s writing [here](http://etherealmind.com/really-need-investment-protection-network/) and [here](http://etherealmind.com/now-two-design-methods-data-centre/).

But as Greg points out, we now have other options. What’s wrong with the chassis-based switches though? I see the issues as risk, flexibility and cost:

* **Risk**: If you have a small number of chassis at the heart of your network, any upgrade has a huge impact, and carries massive risk. You have a large failure domain. You probably also can’t afford a realistic test lab.
* **Flexibility**: Because of the high initial price, there’s an assumption that you will need to maintain this platform in your network for at least 5–7 years. What happens over the next few years as bandwidth goes up and new hardware gets released? You can’t easily change to a cheaper/faster/better alternative, because you need to keep paying off your investment. You can only use the new line cards and supervisors that your vendor releases. The chassis manufacturers do some amazing things to keep old platforms going, but at some point you need to declare the parrot dead.
* **Commercial tension**: If you have a half-populated chassis, what are you going to fill it with? You’ve only got one choice, and the vendor knows it. Vendors will talk about investment protection, but whose investment gets protected? Yours, or theirs?

## They’re not all bad

This is not to say that they are never appropriate. If you have very high port densities, and are purchasing 10+ mostly populated chassis up front, they can be a good option. But most small-medium customers will never need that many ports, and are more likely to only ever buy 2 or 4 chassis.

> But what about all our coming growth?

Where’s your growth coming from? More physical ports, or higher bandwidth per port? If you’re going to need a lot more physical ports in the next 12 months, then a chassis may be appropriate. But for most Enterprises, it’s about fewer ports, running at higher bandwidth levels. Traffic patterns have shifted, and will continue to shift as we figure out better ways of running networks. Isn’t it better to have a design that lets you adapt, rather than staying stuck with a rigid design?

## What’s The Alternative Then?

Leaf-spine architectures seem to be the best alternative. Depending upon your application requirements, you might run either L2 ECMP or L3 ECMP. Controller-based networks & network overlays are re-shaping the picture here too.

Advantages of this approach:

* Lower risk/impact during outages & upgrades.
* Flexibility - can change vendors/models as better alternatives come along. Can replace equipment on 2–3 year lifecycle - similar to servers.
* Buying a lab network is affordable

There is one wrinkle: chassis-based switches have fewer management points than using many smaller switches. What’s the best way to operate a leaf-spine network?

* Traditional protocols and manage as individual devices - hopefully with some automation!
* Controller-based fabrics using something like [Big Cloud Fabric](http://www.bigswitch.com/sdn-products/big-cloud-fabric) or OpenDaylight
* A distributed control system, such as something like [Pluribus](http://www.pluribusnetworks.com/) is doing?

The first option is a viable, proven approach that will give you flexibility, but it does have some limitations. There was a lot of discussion amongst [Network Field Day 8](http://www.techfieldday.com/events/nfd8) delegates about which of the alternative approaches provides a better long-term path. Like most people, I’m still trying to figure it out. I plan on writing more in future about some of the alternatives, their advantages, and their disadvantages.
