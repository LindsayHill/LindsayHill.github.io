---
author: lindsay
date: 2015-12-02 05:24:46+00:00
layout: post
slug: brocade-vdx-snmp-changes
title: Brocade VDX SNMP Changes
categories:
- NMS
tags:
- Brocade
- snmp
---

Brocade tightened up some SNMP settings with NOS 6.0.x. This improves security, but it also means that you will need to modify your configuration if you upgrade. If you don't, SNMP won't work, and you'll get errors with BNA/Nagios/Cacti/etc. Here's the changes, and how to get SNMP working with NOS 6.0.x. NB This applies to [VDX Data Centre switches](http://snmp-server contact "Field Support." snmp-server location "End User Premise." snmp-server sys-descr "Brocade VDX Switch." snmp-server community ConvergedNetwork snmp-server community OrigEquipMfr rw snmp-server community "Secret C0de" rw snmp-server community common snmp-server community private rw snmp-server community public snmp-server user snmpadmin1 groupname snmpadmin snmp-server user snmpadmin2 groupname snmpadmin snmp-server user snmpadmin3 groupname snmpadmin snmp-server user snmpuser1 snmp-server user snmpuser2 snmp-server user snmpuser3). Other product lines have different configuration.

{% include note.html content="Usual disclaimers apply: Yes, I work for Brocade. Doesn't mean that I'm an official spokesperson, or a replacement for TAC. I'm just putting this info out there to help others who get bitten by this." %}




## 5.x and earlier defaults



NOS 5.x and earlier had default SNMP settings that looked like this:


```text
snmp-server contact "Field Support."
snmp-server location "End User Premise."
snmp-server sys-descr "Brocade VDX Switch."
snmp-server community ConvergedNetwork
snmp-server community OrigEquipMfr rw
snmp-server community "Secret C0de" rw
snmp-server community common
snmp-server community private rw
snmp-server community public
snmp-server user snmpadmin1 groupname snmpadmin
snmp-server user snmpadmin2 groupname snmpadmin
snmp-server user snmpadmin3 groupname snmpadmin
snmp-server user snmpuser1
snmp-server user snmpuser2
snmp-server user snmpuser3
```


Yeah. Pretty open. So if you're lazy, and your NMS tried a default discovery string of 'public', you could get SNMP working without touching the config. Naughty.



## 6.x: Sensible defaults



NOS 6.0.x changes the defaults to this:


```text
snmp-server contact "Field Support."
snmp-server location "End User Premise."
snmp-server sys-descr "Brocade VDX Switch."
snmp-server enable trap
```


Much better - no more default strings in use.



## What about upgraded systems?



So what happens if you upgrade? According to the documentation:

> After an upgrade to Network OS6.0.0, the default SNMP configuration from previous releases is removed only if the copy default-config startup-config command is issued before the upgrade. **Otherwise, the running configuration is not changed. (emphasis added)**

However, there is a catch: SNMP views are now required. If you don't configure an SNMP view, SNMP won't work, even with your previous settings.

Add this config snippet to get basic SNMPv2 working with community string 'public':


```text
snmp-server community public groupname user
snmp-server view All 1 included
snmp-server group user v2c read All notify All
```


That creates a new SNMP view that contains the entire tree, from 1 downwards. The configuration maps the community string to the group, which is then mapped to the view. Adding that will get read-only SNMP working the way it did previously. If you need more complex views, you probably already configured them, and weren't affected by this change.

Obviously I don't recommend the use of 'public' nor do I recommend v2 in general. But hey, that's what most people use. You can see what you need to change for your own community string. It will also need further changes for SNMP read-write, and for v3.
