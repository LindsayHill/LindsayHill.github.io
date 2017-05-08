---
author: lindsay
date: 2014-11-04 23:36:44+00:00
layout: post
slug: using-firewalls-for-policy-has-been-a-disaster
title: Using Firewalls for Policy Has Been a Disaster
categories:
- Security
tags:
- policy
- SDN
---

Almost every SDN vendor today talks about policy, how they make it easy to express and enforce network policies. [Cisco ACI](http://www.cisco.com/c/en/us/solutions/data-center-virtualization/application-centric-infrastructure/index.html), [VMware NSX](http://www.vmware.com/products/nsx/features), [Nuage Networks](http://www.nuagenetworks.net/products/), [OpenStack Congress](https://wiki.openstack.org/wiki/Congress), etc. This sounds fantastic. Who wouldn't want a better, simpler way to get the network to apply the policies we want? But maybe it's worth taking a look at how we manage policy today with firewalls, and why it doesn't work.

In traditional networks, we’ve used firewalls as network policy enforcement points. These were the only practical point where we could do so. But…it’s been a disaster. The typical modern enterprise firewall has hundreds (or thousands) of rules, has overlapping, inconsistent rules, refers to decommissioned systems, and probably allows far more access than it should. New rules are almost always just added to the bottom, rather than working within the existing framework - it's just too hard to figure out otherwise.

Why have they been a disaster? Here's a few thoughts:


  * Traditional firewalls use IP addresses. But there's no automated connection between server configuration/IP allocation and firewall policies. So as servers move around or get decommissioned, firewall policies don't get automatically updated. You end up with many irrelevant objects and rules.

  * Object grouping looks great...but it needs an iron first to enforce it. Most systems end up with many groups that look very similar, but are just a little bit different. Or they are inconsistently used - sometimes it's groups, sometimes a rule has 20+ individual objects listed.

  * Exceptions. So, so many exceptions. Is this because the high-level policy wasn't realistic, or because people don't understand it?

> "No plan of operations extends with certainty beyond the first encounter with the enemy's main strength" (or "no plan survives contact with the enemy")

_[Helmuth von Moltke the Elder](http://en.wikipedia.org/wiki/Helmuth\_von\_Moltke\_the\_Elder)_

  * Carelessly adding objects to groups - I've seen large networks get added to groups, without any thought to what the wider implications might be. Suddenly the policy is far more open than ever intended.

  * Network engineers are disconnected from application engineers, leading to inconsistent change requests. e.g. a typical policy might say:  
"Web servers -> App servers -> TCP/8000 - ACCEPT"  
No problems, except then a change request comes through for:  
"Subset(web servers) -> Subset(app servers) -> TCP/8001"  
So what do you do? Add a new rule? Or update the previous rule to also allow 8001? If you add a new rule, chances are you'll probably get another request come through next week, asking to add TCP/8001 access to more of the app servers. An experienced engineer will know what needs to be done. But a junior engineer will just keep adding to the sprawl.

  * Network engineers don't understand applications, and don't challenge incorrect requests - e.g. I have seen so many firewalls where a rule will say something like "NMS Server -> Network Devices -> SNMP, SNMP-TRAP - ACCEPT." It only takes a basic understanding of SNMP to know that the SNMP and SNMP-TRAP rules will never be in the same direction.

  * Careless use of bi-directional rules. A request might say "Server A must be able to SSH to 10.0.0.0/8. All networks should be able to SSH to 10.0.0.0/8." So the engineer adds this rule:
"Server A, 10.0.0.0/8 -> Server A, 10.0.0.0/8 -> SSH - ACCEPT".
Can you see the problem? Now you've allowed 10.0.0.0/8 -> 10.0.0.0/8 SSH, which probably wasn't what you wanted.


Will things be better with future platforms for expressing policy? Maybe. I'm cautiously optimistic. They should provide better abstractions, and more flexible models. We should avoid the IP address problem, and clearer policy language might help avoid some implementation mistakes. But they still risk turning into a big pile of spaghetti.
