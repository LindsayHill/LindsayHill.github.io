---
author: lindsay
date: 2013-10-02 04:10:30+00:00
layout: post
slug: hp-imc-7-0-roundup
title: HP IMC 7.0 New Features Roundup
categories:
- HP IMC
tags:
- imc
- NMS
- upgrades
---

HP has released Version 7.0 of [Intelligent Management Center](http://h17007.www1.hp.com/us/en/networking/solutions/network-management/index.aspx). This is a significant upgrade from 5.2, and greatly modernises the interface. (In case you're wondering, version 6 was never publicly released. Seems IMC number is matching Comware numbering?). While most workflows are identical, the look and feel is quite different. Here's the highlights of the new features:

* New HTML5 interface, with client-side Java mostly banished
* Mobile device interface
* Cisco Wireless equipment support (!)
* Hyper-v 2012, Windows 2012, RHEL 5.9, 6.4, MySQL 5.6 support
* Configuration - better software upgrade support
* Compliance - improvements in policy handling
* Alarm Management - Delete/Acknowledge/Recover all alarms in one go
* Use interface Alias or Description in alarms
* Performance - can modify instance name

For a full list of new features, check out [the Release Notes](http://h20564.www2.hp.com/portal/site/hpsc/public/kb/docDisplay/?docId=c03930380). Downloads here for [Standard](https://h10145.www1.hp.com/downloads/SoftwareReleases.aspx?ProductNumber=JF377A) and [Enterprise](https://h10145.www1.hp.com/downloads/SoftwareReleases.aspx?ProductNumber=JG748AAE). Let's go through some of the changes in more detail.

## New Interface - HTML5

This is the biggest change, that you'll notice right from the login page. All the icons and visuals have changed, although the workflow is mostly the same. Most (but not all) client-side Java has been replaced by HTML5. Browser support has changed - it's now IE 9/10, Firefox 20+, and Chrome 26+. There's even themes for applying a different colour scheme. Take a look at some of these screenshots:

[![IMC 7.0 Homepage](/assets/2013/10/imc_7_homepage.png)](/assets/2013/10/imc_7_homepage.png)

{:.image-caption}
IMC 7.0 Homepage

[![IMC 7.0 Resources](/assets/2013/10/imc_7_resources.png)](/assets/2013/10/imc_7_resources.png)

{:.image-caption}
IMC 7.0 Resources

When looking at a Custom View, or IP Topology, you can now view the Topology using HTML5, and not Java:

[![IMC 7.0 Topology Custom View using HTML5](/assets/2013/10/imc_7_topology.png)](/assets/2013/10/imc_7_topology.png)

{:.image-caption}
IMC 7.0 Topology Custom View using HTML5

Unfortunately, If you go Resource -> Network Topology, the pop-up still uses Java. I don't understand why Java still lingers for this Topology view, when it's been banished from the other views. One step at a time I guess.

Clicking around the interface, you'll see a lot of different icons, and improved behaviour. It's still a touch rough though - sometimes text doesn't line up with boxes quite right. Plus it still does that annoying thing where clicking a menu item will take you to that page, but the menu boxes on the side pane won't update to show your current location. They still suppress right-click too - very frustrating when you want to right-click and copy something. Use Ctrl+C instead.

## Mobile Interface

IMC had IOS and Android applications that could be installed on your phone, to provide basic views of your network status. The application felt unloved, and never saw any updates. But now that they've moved to HTML5, you don't need the application - you can just browse directly to the site. It's a pretty basic view, but it will show you your devices and alarms, and you can ping a node. Not much else.

[![Mobile View Frontpage](/assets/2013/10/IMG_0100.png)](/assets/2013/10/IMG_0100.png)

{:.image-caption}
Mobile View Frontpage

[![Device Details View](/assets/2013/10/IMG_0101.png)](/assets/2013/10/IMG_0101.png)

{:.image-caption}
Device Details View

[![Mobile View Alarm List](/assets/2013/10/IMG_0102.png)](/assets/2013/10/IMG_0102.png)

{:.image-caption}
Mobile View Alarm List

[![Mobile View Alarm Details](/assets/2013/10/IMG_0103.png)](/assets/2013/10/IMG_0103.png)

{:.image-caption}
Mobile View Alarm Details

## Platform Support

This release supports Windows 2012, RHEL 5.9 and 6.4. It also supports MySQL 5.6, and IMC has supported MS SQL Server 2012 since 5.2 SP1. I don't know any customers yet that **require** Windows 2012 support, but I expect that to change within 12 months. Good to see that they're not lagging behind there.

## Configuration and Compliance

Configuration Management has seen a couple of improvements around device support. SCP is supported for ProCurve software management, along with ISSU upgrades for Comware v7. Looking through the Expect scripts, a lot of the code has been changed to use "send -s", to slow down the sending of commands. This will probably help with some of the issues I've seen where it seems to overrun buffers.

Compliance has had some minor workflow improvements. Previously, if you only wanted to run a single Compliance Policy check, you had to go through and individually disable all the other policies. Now you can disable them all in one go. Compliance Task Management is still painful in that you can't modify a scheduled task, or even have it apply newly updated rules, but it is slowly getting better. I remain hopeful.

## Alarm Management

You can now Delete/Recover/Acknowledge ALL alarms in one go now. Previously you could only do it for all alarms displayed on the screen - a maximum of 200. This will be good news to anyone who has had to deal with an event storm.

You can also use Interface Aliases in Alarm description field - it's much more helpful to see "Interface 'Telstra MPLS Link CCT: 223345' Down", rather than "Interface Gig0/1 Down".

For those with hierarchical IMC systems, if you recover an alarm, it now also recovers it in the upper- or lower-level IMC system.

If you drill through an Interface Details page, you'll see a list of alarms for that interface - previously this only worked at the device level.

## Other Improvements and Fixes

* Cisco wireless infrastructure support in WSM - haven't had a chance to play with this yet, but it looks promising.
* Auto-discovery can now use filtering to only add specific device models. I'd prefer to be able to filter OUT specific device classes - e.g. printers - but this is a start
* Performance Monitor Instances can now have their name modified.
* Performance Monitoring - change thresholds from performance graph
* Topology - customise icon for a single device (as opposed to entire device class)
* Device import from CSV - can now specify Custom View

## Licensing Changes

HP has changed the license allocations and block sizes for IMC. Previously IMC Standard came with 100 nodes included in the base license, while IMC Enterprise came with 200 nodes. The smallest additional license block was 100 nodes.

That has now changed - both Standard and Enterprise ship with 50 nodes, and you can buy 50-node blocks. NTA (NetFlow/sFlow) licenses previously started at 10 nodes, now they start at 5 nodes.

Pricing has been updated to reflect changed node counts. This means that starting points are much lower, particularly for Enterprise. Steps are smaller too, but the overall costs are the same - if you previously needed say 400 nodes, the overall price will be the same - but you might be able to get away with buying 350 nodes, instead of 400. It gives you just a little more flexibility.

UAM licensing has also changed - it is licensed based on concurrent users, not users in the database - this can have a very large impact on your required license numbers.

I believe existing customers should be able to use their current licenses, with no changes, but I haven't had a chance to test this. Quite important to know beforehand, if you don't have active support.

## Conclusion

This release is a really positive move for HP. It doesn't fix everything I'd like, but the interface was well overdue for an overhaul. Their regular release cycle is pleasing too - we're seeing real, usable improvements with each release. The product is a LONG way ahead of where it was two years ago. That said, I'm not quite ready to recommend production use. I need to test it out more in my lab, I highly recommend you do too.

{% include note.html content="Disclaimer: HP gave me early access to IMC 7.0, but did not ask me to write anything, nice or otherwise." %}
