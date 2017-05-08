---
author: lindsay
date: 2013-06-29 03:24:52+00:00
excerpt: It can be hard to understand the differences between HP's NNMi and IMC. Both
  offer a great range of network management capabilities. Here I outline the technical
  similarities and differences between the products.
layout: post
slug: imc-vs-nnmi
title: HP IMC vs HP NNMi - Technical Differences
categories:
- NMS
tags:
- hp
- imc
- nnmi
---

[HP NNMi](https://saas.hpe.com/en-us/software/network-node-manager-i-network-management-software) and [HP IMC](https://www.hpe.com//networking/imc) are both very capable network management products. But to an outsider, it can be a bit hard understanding the differences between the products - why does HP offer two overlapping products? I can't answer that fully, but I can try to show some of the technical differences between the products, to help you understand the similarities and differences.

**Update:** I've posted a follow-up piece that's more opinion-based, and tries to answer the question: "[So which one should I choose]({% post_url 2013-07-01-imc-nnmi-which-one %})?"

| Overview | NNMi | IMC|
|---------+-----------------------+--------------------|
|**Summary** | High-end network monitoring system, for very large networks. Focuses on policies, automatic rules, capabilities above ease of use or look and feel | Mid-range network monitoring system, for a range of network sizes, ideal for monitoring and managing mixed-vendor networks. Simpler to use, with nicer look and feel, but at the expense of some capabilities.
|---------+-----------------------+--------------------|
|**History** | NNMi is the modern successor to HP OpenView - one of the first network management tools on the market, many years ago. Completely rewritten in 2008, with a new name (NNMi), interface, discovery and polling engine. R&D team based in the US, with development and support teams in India. It is part of the HP Software group, and has strong ties to HP's BSM Software suite. Current version of NNMi is 10.00. | IMC was originally developed by 3Com, for managing large 3Com/H3C networks in China. After the HP acquisition, it was brought into the HP fold, but has stayed with the HP Networking group (as opposed to HP Software). It now supports HP ProCurve systems, and a variety of other vendors, including Cisco. R&D, development and support are done from China. Current version is 7.1 | 
|---------+-----------------------+--------------------|

---

|Standard Features|NNMi|IMC|
|---------+-----------------------+--------------------|
|Fault Management|Yes - with Root Cause Analysis, hardware monitoring for supported systems, etc.|Yes - but limited Root Cause Analysis or default hardware monitoring (can be configured).|
|---------+-----------------------+--------------------|
|Network Discovery|Yes - extensive options with continuous spiral discovery|Yes - standard options for scheduled discovery (route-based, ARP cache, ping sweep, etc)|
|---------+-----------------------+--------------------|
|Topology Mapping|Yes - L2 or L3 connectivity maps. Manual links require CLI configuration.|Yes - including rack/datacentre layout. Manual links can be added via GUI.|
|---------+-----------------------+--------------------|
|Performance Reporting|Yes, with iSPI Performance for Metrics add-on. More reporting now integrated into main product, making it easier to use. Advanced reporting is possible, but still challenging.|Yes - in base platform. Far simpler to and quicker to use, but with limitations - can't do deep, advanced reporting.|
|---------+-----------------------+--------------------|
|Configuration Management - Backup, Deploy|Yes, with HP Network Automation (HP NA) add-on.|Yes - in base platform.|
|---------+-----------------------+--------------------|
|Firmware Management|Yes, with HP Network Automation (HP NA) add-on.|Yes - in base platform.|
|---------+-----------------------+--------------------|
|Syslog|No, but can integrate with HP Arcsight.|Yes - in base platform. (nb: this is not a full syslog management solution)|
|---------+-----------------------+--------------------|
|NetFlow|With iSPI Performance for Traffic add-on.|With NTA add-on.|
|---------+-----------------------+--------------------|
|VMware/Hyper-V Integration|No.|Yes - discovery/mapping of VMs and vSwitch in base platform. VAN add-on for policy integration.|
|---------+-----------------------+--------------------|
|Wireless Controller Integration|No.|Yes - with WSM add-on. HP, Cisco and Aruba controllers.|
|---------+-----------------------+--------------------|
|Integration Packs|Standard integrations for HP OM, BSM, Netcool, CiscoWorks LMS, IMC, HP SIM, HP SiteScope, HP UCMDB.|No standard integration packs, other than NNMi integration.|
|---------+-----------------------+--------------------|
|Integration Capabilities|SNMP traps, custom scripts.|Email, SNMP traps.|
|---------+-----------------------+--------------------|
|Device Support|Broad, well documented. Does not appear to favour HPN gear. Supporting new devices may need HP support.|Broad support, but not well documented (publicly). Aims to be open, but tends to support HPN gear first, followed by Cisco, then others. Can easily add support for new devices.|
|---------+-----------------------+--------------------|
|Scalability|Supports hierarchical model with GNM license. Known to manage networks over 60K nodes.|Supports hierarchical model with Enterprise license. Known to manage networks over 20k nodes.|
|---------+-----------------------+--------------------|

---

|---------+-----------------------+--------------------|
|Advanced Features|NNMi|IMC|
|---------+-----------------------+--------------------|
|Custom Reporting|Yes, using Cognos.|Yes - using Crystal Reports (ugh)|
|---------+-----------------------+--------------------|
|Configuration Compliance|Yes, with HP NA.|Yes - base platform.|
|---------+-----------------------+--------------------|
|MPLS|With iSPI for MPLS add-on.|With MPLS VPN Manager module.|
|---------+-----------------------+--------------------|
|BYOD|No.|With UAM module.|
|---------+-----------------------+--------------------|
|TACACS/RADIUS|No.|With TAM add-on.|
|---------+-----------------------+--------------------|
|API|Yes.|Included with Enterprise License, or add-on to Standard License.|
|---------+-----------------------+--------------------|
|Multicast|With iSPI for Multicast add-on.|No.|
|---------+-----------------------+--------------------|
|Telephony|With iSPI for Telephony. Multi-vendor.|With Voice Services Manager module - HP only.|
|---------+-----------------------+--------------------|
|Routing Analytics|With RAMS add-on.|No.|
|---------+-----------------------+--------------------|
|Automated Actions (Runbook Automation)|Yes, with iSPI NET or HP NA.|Limited capabilities with SCC.|
|---------+-----------------------+--------------------|
|Policy-Driven Capabilities|Extensive options for grouping devices/interfaces based on characteristics (e.g. sysLocation, IP address, interface description, device model). Fault/Performance/Escalation policies can then be applied to these groups. With the right rules in place, most monitoring policies are automatic. Problem is that ad-hoc changes are difficult.|Limited abilities, but improving. No ability to automatically apply policies to groups of interfaces. Easy to apply ad-hoc changes though - e.g. manage/un-manage interface, add performance polling, etc.|
|---------+-----------------------+--------------------|

---

{% include note.html content="Last updated: 18th November 2014" %}

