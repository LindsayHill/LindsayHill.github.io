---
author: lindsay
date: 2014-11-12 22:10:36+00:00
layout: post
slug: accurate-dependency-mapping-one-day
title: Accurate Dependency Mapping - One Day?
categories:
- NMS
tags:
- NMS
- snmp
---

Recently I’ve been thinking about Root Cause Analysis (RCA), and how [it’s not perfect]({% post_url 2014-11-04-root-cause-analysis-its-not-perfect %}), but there may be hope for the future.

The challenge is that Automated RCA needs an accurate, complete picture of how everything connects together to work well. You need to know all the dependencies between networks, storage, servers, applications, etc. If you have a full dependency mapping, you can start to figure out what the underlying cause of a fault is, or you can start doing ‘What If?’ scenario planning.

But once your network gets past a moderate size, it’s hard to maintain this sort of dependency mapping. Manual methods break down, and we look for automated means instead - but they have gaps and limitations.


## Automated Mapping - Approaches & Limitations


Tools such as [HP’s CMS suite](http://www8.hp.com/nz/en/software-solutions/configuration-management-system-database/) attempt to discover all objects and dependencies using a combination of network scanning and agents. They’ll use things like ping, SNMP, WMI, nmap to identify systems and running services. Agents can then report more data about installed applications, configurations, etc.

Network sniffing can also be used to identify traffic flows. Most tools will also connect to common orchestration points, such as vCenter, or the AWS console, to discover underlying components.

But you can’t trust the results. Discovery tools can’t get everything - e.g. misconfigured firewalls or SNMP means devices get missed. Automated network analysis will struggle to identify that flow that only happens once a year. You need to put a lot of effort into review the results of automated analysis, and you need to keep tweaking it.

The problem is people, and the tools we use to maintain systems. Automated tools don’t know that you went and installed a router, but screwed up the SNMPv3 user. They might find out that you installed Apache on that server, but they might not understand that new application you've just started using. They don't have one source of truth, and so they have to take a best guess.

Because you can go and do things to applications and systems individually, automated discovery breaks down.


## What about the (Nearly) All Virtual Future?


But what if all change was done through a few orchestration systems? What if I could connect to one controller, and identify all the deployed routers, firewalls, servers, storage devices? What if I could connect to one configuration management system, and see what configuration is deployed to my systems? What if I could trust that configuration management system, and know that it was the source of truth? If someone changes an underlying device, that change should get over-written.

That’s the direction we’re going in. And so that’s why I’m hopeful that automated dependency mapping may be reliable in future. Sure, there will still be gaps. But they should be a lot smaller.
