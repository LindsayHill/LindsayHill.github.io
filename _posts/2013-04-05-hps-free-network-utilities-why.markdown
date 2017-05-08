---
author: lindsay
date: 2013-04-05 05:05:36+00:00
layout: post
slug: hps-free-network-utilities-why
title: HP's Free Network Utilities - Why?
categories:
- NMS
tags:
- hp
- network
- nnmi
- tools
---

HP Software recently released [6 free network utilities and tools](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1171412&jumpid=ex_r11374_us/en/large/eb/go_nmc#tab=TAB2). Free stuff is good right? We all like free. But HP doesn't do things out of the goodness of its heart - what's the angle for them to make money? I can't work it out. Recently I tried out these tools, and I can't understand their strategy.

The six tools are:


  * **Ping Your Network** - pings in each host in a given subnet

  * **Network Port Scanner** - scan an IP, tell you open/closed ports

  * **VM Reporter** - connect to vCenter, show hosts, VMs and relationships

  * **Network Device Configuration Inspector** - pull Cisco device configuration. Can compare two devices

  * **User Registrations Viewer for Microsoft Lync** - show currently connected Lync users, client versions, etc.

  * **Network Flow Analytics** - simple NetFlow data analysis and reporting



## Product Test Drive


I had a few moments to try the first four of these recently. Overall I was not impressed, and I don't understand what HP is doing with these. None of them have any obvious tie-in to HP's commercial products, no similarities with interfaces, and all of them are feature-limited, some so much so as to be unusable.

**Ping Your Network**: This is pretty basic. It's quite slow, serially pinging each IP in a subnet. It doesn't even discard the subnet and broadcast IP addresses. The returned data is only useful if you export it to CSV, as you can't manipulate/re-order the data in the GUI. GUI looks nothing like any other HP product that I've seen.

[![HP Ping Your Network](/assets/2013/02/hp_ping_your_net.png)](/assets/2013/02/hp_ping_your_net.png)

{:.image-caption}
HP Ping Your Network

**Network Port Scanner: **Gave up on this after 15 minutes scanning one IP. Still hadn't completed. Can only define a single IP, and a port range. Doesn't do any fingerprinting. Doesn't even list the results in numerical order. There is absolutely no reason for any engineer to use this product, when there are exceptional free products available like [Nmap](http://nmap.org).

[![hp_port_scanner](/assets/2013/02/hp_port_scanner.png)](/assets/2013/02/hp_port_scanner.png)

{:.image-caption}
HP Network Port Scanner

**VM Reporter:** This tool does what it says - it shows you the data centers, hosts and VMs defined in vCenter, and the relationships between them. It also shows you any current alerts. But you look at this tool, and ask yourself: When would I use this? I need login access to vCenter. Why not just login? It doesn't save me any time, or give me any extra information. What am I getting out of it?

[![hp_vm_reporter](/assets/2013/02/hp_vm_reporter.png)](/assets/2013/02/hp_vm_reporter.png)

{:.image-caption}
HP VM Reporter

**Network Device Configuration Inspector:** This tool was perhaps the most disappointing. On starting this, you get a command shell, where you can enter commands. First you define your editor (e.g. Notepad++), then your configuration comparison tool (e.g. WinDiff). You then define credentials, and tell it to either show you a single configuration, or compare configurations. You can only compare two different devices - you can't compare versions of the same device. It ONLY works with Cisco devices, and ONLY works with Telnet and TFTP. Give me a useful GUI, and maybe I'll use it for looking at device configurations. But otherwise, it's just as easy to open a Putty session to a router. If needed, I can slap a couple of configs into WinDiff. I don't really need a tool just to pull configs for me, unless it's got a bunch of automation associated with it. No picture for this one, since you've all seen Notepad and WinDiff before.

None of these tools use any GUIs consistent with the commercial products, nor do they offer any obvious tie-in/upgrade path. The other issue is that the sort of person who downloads a free product probably can't afford the enormous prices HP charges for some of its Network Management products. What is the strategy here?


## Contrast with Virtualization Performance Viewer


Compare this approach with HP's "[Virtualization Performance Viewer](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1270035) (vPV)" - this is a tool for performance management and diagnostics for VMware and Hyper-V. Gives you a quick overview of your VMs, and identifies any that may have performance problems. The free version is limited to 200 VMs, only retains data for 1 day, and doesn't support authorization or reporting. But it's the same tool, and you can quickly see what it does, try it out, and if you like it, there's a clear path to the full version. This is the sort of strategy I can understand.


## Conclusion


It seems like someone in the marketing department decided HP should offer free tools, to entice people into their mega-expensive products. Maybe that's a good strategy, maybe it's not. Some groups within HP understood the strategy, and released something that made sense (vPV). The NMC group decided they didn't want to miss out on the fun, so they rushed something out - but they just didn't think it through properly.
