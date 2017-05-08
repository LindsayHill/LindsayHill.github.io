---
author: lindsay
date: 2014-12-12 08:45:28+00:00
layout: post
slug: big-switch-chaos-monkey-network-testing
title: Big Switch Chaos Monkey Network Testing
categories:
- Routing &amp; Switching
tags:
- NFD
- SDN
---

Whenever you build a complex system, you need to test that it works as expected, including properly handling failures. It's easy enough to do simple component failure testing, but harder to do rapid automated failure tests. [Big Switch](http://www.bigswitch.com/) is showing that it can be done though. Hopefully we can keep improving our testing to pick up some more of the software failures.


## Testing is hard


Over the course of my career I’ve built many clustered systems - [HP-UX Serviceguard](http://www8.hp.com/us/en/products/server-software/product-detail.html?oid=4162060), firewalls, routers, load balancers, RedHat Clusters, etc. Good clusters have redundant everything - servers, power supplies, disks, NICs, etc.

The commissioning process always included testing. We’d go through each of the components, trying to simulate failures. Unplug each of the power cables, the network cables, unseat a hard drive, remove a hot-swappable fan, etc. That would test out the redundant components within each server, and then of course you’d simulate a complete system failure, forcing full failover.

This is all important stuff, but it doesn’t pick up all the failures - e.g. What happens if you’ve got a faulty patch lead, and the link starts flapping? Sometimes a simple failure gets messy when it happens repeatedly over a short period. The other problem with this testing is that it’s pretty manual, and easy to forget something unless you’re following a good checklist.


## Chaos Monkey in the Network


I’m pleased to see that [Big Switch](http://www.bigswitch.com/) is [taking their testing to the next level](http://go.bigswitch.com/rs/bigswitchnetworks/images/Chaos%20Monkey%20and%20Big%20Cloud%20Fabric.pdf), by bringing Chaos Monkey into their testing strategies. Big Switch and [Mirantis](http://www.mirantis.com) ran a Chaos Monkey-style test with 16 racks of gear:


> Despite adding 42,000 simulated VMs to the network and then forcing failures to the Big Cloud Fabric SDN controllers every 30 seconds, forcing failures to random switches every 8 seconds and random links in the fabric every 4 seconds, there was no change in the application performance.


More info [here](http://go.bigswitch.com/rs/bigswitchnetworks/images/Chaos%20Monkey%20and%20Big%20Cloud%20Fabric.pdf) (Warning: PDF).

This is a great result. We don't always hear much about this sort of testing. I know that other vendors do carry out their own testing programs, but we don't see this sort of information. We have to assume it's happened.


## ‘Clean’ failures are easy. ‘Grey’ failures are hard.


‘Clean’ failures are reasonably straightforward to deal with. Software or hardware can quickly and clearly identify the failure, and switch over to backup systems. Dealing with weird software failures is much harder. Consider a situation where a rogue process maxes out the CPU on a primary server. It might just have enough capacity left to send out clustering packets, but it can’t perform its normal functions. The secondary system doesn’t know it should take over, and so the application fails. Can you test those sorts of scenarios, and how do you go about doing it?

Unfortunately those are the ones that tend to bite you hard in production. We can’t always work around them either. We can add more test cases to try to test what happens during that event, and try adding more monitoring and compensating controls. Hopefully we don’t get bitten twice by the same thing.

{% include note.html content="Big Switch Networks was a sponsor of [Network Field Day 8](http://techfieldday.com/event/nfd8/). This meant that they indirectly covered some of my costs to attend. However, I am under no obligation to write anything about any of the presenters - I just found this topic interesting." %}
