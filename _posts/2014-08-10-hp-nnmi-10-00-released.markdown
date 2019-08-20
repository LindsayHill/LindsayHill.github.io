---
author: lindsay
date: 2014-08-10 02:29:38+00:00
layout: post
slug: hp-nnmi-10-00-released
title: HP NNMi 10.00 Released
categories:
- HP NNMi
tags:
- hp
- nnmi
---

HP NNMi version 10.0 has been released. This is a good release, with many usability enhancements. I’m pleased to see continued development, as the future nirvana of all-powerful software defined networks hasn’t quite arrived yet. For now, we still have to manage our networks the old-fashioned way: SNMP is still alive & kicking.

## NNMi - Background

[HP NNMi](https://saas.hpe.com/en-us/software/network-node-manager-i-network-management-software) is a spiritual descendant of HP OpenView, one of the first network monitoring tools. Between versions 6 and 7, HP completely re-wrote the NNM code, and now we have NNMi. The core product performs network discovery and fault monitoring. Add-on components (iSPIs) offer performance monitoring, NetFlow analysis, IP SLA monitoring, etc. A sister-product (HP Network Automation) is used for network configuration management. The add-on components were all separately licensed, but HP now [bundles products together]({% post_url 2014-02-03-hp-nnmi-licensing-changes %}).

Historically NNMi has focused on underlying network monitoring capabilities, and less on the user interface. This meant that almost anything was technically possible, but the visual experience was _underwhelming._ The integration between core product and add-on components was limited.

The last major release was 9.20, in June 2012. There have been minor enhancements and fixes since, but the last patch was in September 2013. We’ve been due for an update.

## Key Features

There’s three broad strokes - improvements to usability, cleanup of old code, and better component integration. Here’s a quick list of the most interesting changes:

* Dashboards - key status and performance indicators at a glance
* Scheduled Outages
* Improved chassis/stack management
* Support for VDCs and server LAGs
* IPv6 on by default, on all platforms
* iSPI Performance can be distributed across multiple systems
* Platform changes - dropping Solaris, HP-UX & RHEL 5.x support
* Removal of legacy integrations
* Updated browser support
* Map annotations - add text to maps

Drilling into a few of those:

### Dashboards

The most obvious change is adding dashboards. Dashboards pull together many different status and performance indicators for a node or group of nodes. From one page you can now get a good sense of the health of the network - top interfaces, systems with faults, highest CPU, etc. All that information was available separately before, but you couldn’t pull it together in one place.

Here’s some example screenshots:

[![Network Overview Dashboard](/assets/2014/08/Network-Overview-Dashboard.png)](/assets/2014/08/Network-Overview-Dashboard.png)

{:.image-caption}
Network Overview Dashboard

[![Component Performance Dashboard](/assets/2014/08/Component-Performance-Dashboard.png)](/assets/2014/08/Component-Performance-Dashboard.png)

{:.image-caption}
Component Performance Dashboard

I haven’t been able to show all the widgets on each dashboard - they scroll down beyond one screen here.

This version has limited dashboard customisation capabilities, but I expect that will change.

### Scheduled Outages

In earlier versions, you can mark a node as “Not Managed” or “Out of Service.” This would stop any alarms for that node - helpful if you’ve got planned work, or a known outage. The problem is that it was easy to mark a node as out of service…and then forget to re-enable monitoring in future. You might never know there’s anything wrong…until something breaks, and people start asking why they didn’t get any alarms.

Now you can configure a scheduled outage, either for planned work, or when there’s a known outage that you expect to be restored shortly. You can also configure _retrospective_ outages, so the downtime won't count against your SLAs.

[![Scheduled Outage Configuration](/assets/2014/08/Schedule-Outage.png)](/assets/2014/08/Schedule-Outage.png)

{:.image-caption}
Scheduled Outage Configuration

It doesn’t look like you can scheduled recurring outages, but they can be configured via CLI, so you could script it.

### Platform Changes

NNMi 9.20 was supported on Solaris, HP-UX, RHEL 5.x & 6.x, SUSE Linux and Windows 2008 R2. Historically the customers that used OpenView/NNM were the customers that ran Solaris/HP-UX Big Iron systems. But those systems have been mostly supplanted by Linux. These days there is little reason not to run on a Linux VM. There are standard migration paths to move from Solaris/HP-UX -> Linux.

Removing these legacy platforms makes little difference to customers, but it will make a huge difference to development, test and support. Consolidating platforms will cut costs, and should speed up future development.

## Verdict

This is a significant release for NNMi. They’ve taken significant steps to improve product usability. They're also starting to deliver on the promise of “fully integrated network management.” They’re not quite there yet, but they’re getting closer. I’m also pleased with the changes that remove legacy platforms and integrations. Removing old code makes future test & development cycles simpler and faster.

It’s a major release, so I would hold off upgrading existing installations until patch 1. But I do recommend running it in new deployments, and in your test environment. Note that you will need to re-license NNMi if you are upgrading (no charge if you have a support contract).
