---
author: lindsay
date: 2013-06-21 17:00:41+00:00
excerpt: Recently I upgraded an HP SIM system from 7.1 to 7.2 on Windows 2008 R2.
  After the upgrade, SNMP discovery and data collection failed for all systems - servers,
  switches, blade enclosures, etc. WMI monitoring/discovery of Windows-based systems
  was working, and SNMP traps were being received, but SNMP polling was failing. Here's
  how I diagnosed and fixed it.
layout: post
slug: hp-sim-7-2-snmp-fails
title: HP SIM 7.2 Upgrade - SNMP polling fails
categories:
- NMS
tags:
- hp
- SIM
---

Recently I upgraded an [HP SIM](http://h18013.www1.hp.com/products/servers/management/hpsim/index.html) system from 7.1 to 7.2 on Windows 2008 R2. After the upgrade, SNMP discovery and data collection failed for all systems - servers, switches, blade enclosures, etc. WMI monitoring/discovery of Windows-based systems was working, and SNMP traps were being received, but SNMP polling was failing. Here's how I diagnosed and fixed it.

During the upgrade, I manually upgraded to the latest SQL Server 2008 Service Pack. I then ran the SIM upgrade tool, and it completed without error. Once that had completed, I restarted the server, and tested it out. I found that System Identification tasks were having errors - SIM was saying that it could not communicate with devices via SNMP. I knew that this had worked before, and had the logs to prove it. I could also see that SNMP identification/discovery was working after the DB was upgraded, but before the application was upgraded.

Intrigued, I did what any good network engineer would do, and brought out Wireshark. Looking at the SNMP traffic, I could see requests going out, with the correct community string, and replies being received. So if it's not the community string or ACLs, and clearly routing is working, then what's going on? Looking at the timestamps, I saw that SIM was sending out an SNMP get-request, waiting 5s, repeating the request, waiting another 5s, then moving on to the next task. Looking at the protocol settings, this is the default for SIM - wait 5s, and retry once. All requests had matching replies being received, why was SIM not processing them?

I started to wonder about issues such as firewalls, but the Windows firewall was disabled on this system. I guessed it had to be some change based upon the upgrade, so I started digging into it. I came across another issue where all inbound SNMP traps were creating XML files in the <SIM>/traps directory. I had over 20,000 XML files already. This is apparently addressed by a hotfix that has just been released, but it does not address my SNMP problem.

SNMPv3 support was introduced with SIM 7.2, so I wondered if this was related. I hadn't configured any SNMPv3 credentials, but it could still have been an issue. So I disabled SNMPv3 support, but no change.

I had taken a backup of the SIM install directory before starting, so I had something to fall back on. The first thing I did was to look at the main configuration file - globalsettings.props. I compared the original and upgraded files. There were a few new entries - as could be expected with the new features. But one changed line got my attention - the value of `JavaSNMPStack` changed from `false` to `true` when upgraded. I couldn't find any documentation anywhere that explained what this did. But I tried changing it back to false, and restarting SIM. Bingo! SNMP polling starting working again.

I have since received confirmation from support that this was the right course of action. Hopefully it doesn't happen again with the next upgrade!

**UPDATE:** I have just applied the May Hotfix to SIM. After this, JavaSNMPStack was set back to true - but this time it seems to be working. Applying this hotfix should be your first course of action now. Also note that you need to edit globalsettingsupd.tpl to make changes to globalsettings permanent across patches.
