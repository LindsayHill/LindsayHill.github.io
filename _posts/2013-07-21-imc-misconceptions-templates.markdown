---
author: lindsay
date: 2013-07-21 07:55:39+00:00
layout: post
slug: imc-misconceptions-templates
title: IMC - Clearing up Misconceptions about Templates
categories:
- HP IMC
tags:
- imc
---

I'd just like to clear up a little confusion that people have about [HP IMC](http://h17007.www1.hp.com/us/en/networking/products/network-management/IMC_SS_Platform/index.aspx) templates, and how they are used. Templates can be used to define credentials for different access methods - SNMP, Telnet, SSH, WMI, PowerShell, SOAP. You can define multiple templates for each access method, and they can have a range of options - e.g. Username/Password, Username/Password + enable password, Private Key + enable password, SNMPv3-Priv-Aes128-Auth-Sha, etc.

Each credential combination you create has a name. When adding a new device to IMC, or configuring a discovery schedule, or modifying a new device, you can set the access credentials from your list of previously saved templates. This is tremendously useful if you have some horrendously long password that is used across multiple devices. Rather than re-entering it on each device, you can just choose it from a list of templates. That's all well and good.

There's two area of confusion around this that I see. The first is that some people think that you **have** to use templates. This is only true for SNMPv3. Any other credential (SNMPv2, SSH, Telnet) can be manually entered, if that suits you. This is fine if you've got a one-off device, not so clever if you've got common community strings across your systems.

The second area of confusion is around changing a template. People assume that if you change a template, it will affect all currently configured devices. Again, this is only true for SNMPv3. For all other template types, they are only used when the device is discovered or modified. Future updates to SSH, Telnet or SNMPv1/v2 templates will NOT automatically update the saved passwords IMC has for those devices. Future updates to SNMPv3 templates WILL affect devices using those templates.

Oh. But what if I do want to update the saved passwords for all devices? This is where you can use "Batch Operations." Go to Resource -> Batch Operation. Click on one of "SNMP Settings", "Telnet Settings" or "SSH Settings" - you can then pick a template, and devices to re-apply it to. That will update the saved passwords. Of course, it won't actually update the device itself - you can do that through either Device Configuration, or my preferred method of using the Configuration Center.

Templates will certainly make your life easier, and I highly recommend using them. Just be aware of what they can and can't do.
