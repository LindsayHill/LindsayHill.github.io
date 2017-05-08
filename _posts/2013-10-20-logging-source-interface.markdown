---
author: lindsay
date: 2013-10-20 17:00:27+00:00
layout: post
slug: logging-source-interface
title: Logging source-interface with VRFs
categories:
- NMS
tags:
- cisco
- logs
---

Some Cisco routers I work with use multiple VRFs. A specific VRF is used for management, and the loopback interface is in that VRF. All SNMP + SSH access is via that Loopback interface. This is the Right Way to do it, as it ensures that I can always access the same IP address, regardless of which WAN link is in use. All very good.

Those who use Loopback interfaces will be aware that it is important to ensure that management traffic sourced from the router will use the correct source IP address. Any SNMP or SSH responses will use the right IP address, but when the packet is originated by the router, it will default to using the IP address of the outbound interface. The classic example is SNMP traps and syslog messages from the router. This can confuse most NMSes, as they don't properly correlate the syslog with the IP address that they use for managing the device.

To get around this problem, Cisco introduced commands like this:

```text
snmp-server trap-source Loopback0
logging source-interface Loopback0
```

This tells the router to use the IP address configured on interface Loopback0 when sending out syslogs or SNMP traps. But what if we're using VRFs? When we try and add the logging source-interface command, we get this:

```text
Router(config)#logging source-interface Loopback 0
Interface Loopback0 is not in the global table
Router(config)#
```

If we're particularly unlucky, it just accepts the command, and doesn't display anything to screen, so you think it's OK. It's only when you check the config that you realised the command is missing, and it's sent a message to syslog telling us that it couldn't take that command. OK, can we add "vrf mgmt" that command? Maybe. It depends on what IOS version we're running. Looking at the [Command Reference](http://www.cisco.com/en/US/docs/ios-xml/ios/esm/command/esm-cr-a1.html#GUID-0B44C81B-B061-4467-BDB7-36A9DFF114FD), we see this in the Command History:

> This command was integrated into Cisco IOS Release 15.1(1)SY. The **vrf** keyword and **vrf-name** argument were added.

And what version am I running here? Not 15.1. Still using 12.4T on these. Sigh. Now I'm looking at a heap of upgrades, just to get syslogs properly working with VRFs.
