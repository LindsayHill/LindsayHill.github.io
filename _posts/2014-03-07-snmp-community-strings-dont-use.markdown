---
author: lindsay
date: 2014-03-07 04:00:17+00:00
layout: post
slug: snmp-community-strings-dont-use
title: SNMP Community Strings - Don't Use '@'
categories:
- NMS
tags:
- snmp
---

A quick reminder - do not use the symbol '@' in SNMPv1/2 community strings. I came across this again this week - it causes issues with monitoring some equipment, and should be avoided.

Let's just ignore the massive SNMPv1/v2c security issues for a second. Since people see the community string as a password, it's common to use a random collection of letters, numbers and characters. For some reason, people seem to end up using '@' in their community strings. But look at this in the [Cisco documentation](http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/snmp/command/nm-snmp-cr-book/nm-snmp-cr-s2.html#wp4041297190):

> The @ symbol is used as a delimiter between the community string and the context in which it is used. For example, specific VLAN information in BRIDGE-MIB may be polled using community@VLAN_ID (for example, public@100) where 100 is the VLAN number. Avoid using the @ symbol as part of the SNMP community string when configuring this command.

Cisco has more information about [Community String Indexing](http://www.cisco.com/c/en/us/support/docs/ip/simple-network-management-protocol-snmp/40367-camsnmp40367.html):

> Some standard MIBs assume that a particular SNMP entity contains only one instance of the MIB. Thus, the standard MIB does not have any index that allows you to directly access an instance of the MIB. In these cases, a community string indexing is provided to access each instance of the standard MIB. The syntax is [community string]@[instance number].

If you use '@' in your community strings, you might find that the switch gets confused about what you're trying to poll. You won't be able to get data for any VLAN other than 1, and you'll probably find that you get SNMP authentication failure traps from your managed devices.

Do yourself a favour and use a different character. Maybe '?' for bonus marks in struggling to work out how to enter that via the IOS CLI?
