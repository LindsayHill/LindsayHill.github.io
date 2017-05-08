---
author: lindsay
date: 2014-05-06 06:00:13+00:00
layout: post
slug: hp-imc-7-0-e0202-steady-improvements
title: 'HP IMC 7.0 E0202: Steady Improvements'
categories:
- HP IMC
tags:
- hp
- imc
---

When I'm evaluating products, I'm more interested in their progression and development, than the exact feature list currently shipping. I like products that have a frequent release cycle, with a clear feature roadmap, even if they have a few rough edges. Better that than an OK product that hardly ever sees updates. [HP IMC](https://www.hpe.com/networking/imc) is a product that has been steadily, visibly improving over the last few years, with a release cycle around 6 months. IMC is a key part of HP's overall SDN strategy, and there's been a lot of focus on delivering major new components. But there's also a number of smaller improvements and tidy-ups going on. Here's some of the improvements in the most recent release, [7.0 E0202](https://h10145.www1.hpe.com/Downloads/SoftwareReleases.aspx?ProductNumber=JG747AAE&lang=en&cc=us&prodSeriesId=4176535):



## Functionality Enhancements



  * Custom Views can now have dynamic rules for assigning devices. Previously this could only be done by IP Address range. Now you can assign devices to groups based on device type, sysName, sysLocation, sysContact. This isn't yet perfect - it only seems to apply the rules when devices are discovered, not when you change the group definition. It does support wildcards though.

  * IPv6 management support - initially only Comware devices

  * DBman now raises an alarm if DB backup/copy/restore fails - this would have alerted me to a customer problem MUCH sooner if this feature was available in 5.2.

  * HTML5 topology - this is now an option on the Resource drop-down menu. Previously the only option there was Java-based topology, and the HTML5 view option was buried. You now have a choice of "Network Topology" or "Network Topology (applet)." HTML5 is not yet complete, but it works for more topology views now.

  * VMware/Hyper-V/Xen/KVM - more support for doing things like deploying VMs from template, clone VMs, powering devices off/on, etc. No idea why you would do this from IMC though.

  * Backup plans can be configured per-custom view.

  * Better alarm filtering rules per-custom view.


Unfortunately these improvements allowing more use of Custom Views hasn't yet reached the Compliance Center - Compliance Tasks still need to be configured per-device, or per-device model. You still can't modify a Compliance Task once it's been created. Maybe I'll get lucky with some improvements there in 7.1?



## Cleanups & Fixes



  * Small UI typo fixes (I'm probably the only one that notices)
    
  * Device Adapters - many of the smaller bugfixes I've been maintaining in [GitHub](https://github.com/NetOpsCommunity/imc-netops) have been rolled into the main codebase.
    
  * Added device support - e.g. ASA5520, 5540.
    
  * SSL support for LDAP communications
    
  * Fixed alarming for Link Aggregation members
    
  * No more annoying pop-up if you don't use the Officially Blessed Browsers (Chrome, Firefox, IE 9+).
    
  * Changing the alarm sounds doesn't require a restart
    
  * Ability to link directly to a Device Details page - handy if you're doing integration with other systems.



## Modules & Add-Ons



This release also included upgrades to most of the add-on modules. User Access Management, used for BYOD, has seen a lot of much-needed fixes and enhancements. I haven't had a chance to try them all out, but some of the other changes I liked are:


  * Application Performance Management - added support for PostgreSQL, Hyper-V, VMware, and added an agent for more detailed monitoring.

  * Aruba Wireless Management

  * Wireless topology - some pieces now available in HTML5, although still limited - you can't delete Java just yet.

  * SDN Manager - includes support for App Management. Currently it's all manually imported - shouldn't be too far away before the [HP SDN App Store](http://h17007.www1.hp.com/us/en/networking/solutions/technology/sdn/index.aspx#tab=TAB1) goes live.


The Reporting application (iAR, or Intelligent Analysis Report) doesn't have any updates. Customised reporting continues to be a weak area for IMC, and indeed most network management platforms.


## Recommendation: Upgrade Now


You could argue that some of the above things should have been there already, and maybe they should have. I'm not too worried about that though - what I am pleased by is that the product continues to improve, with many smaller improvements, not just adding major new features.

I'm happy with what I've seen of E0202 in my lab and customer environments. I recommend upgrading now. Note that you can install this version directly, or on top of any version from 5.2 E0401 onwards. You do not need to install earlier releases, except for WSM. I tried doing a clean installation of WSM E0202 in my lab, and it failed, due to database setup issues. This was with MySQL on Linux. Installing WSM 7.0 E0102, and then upgrading WSM worked. You can install the older WSM version with the newer base IMC platform, so it's only WSM that needs to be a two-stage process.

The only reason to not upgrade is if you are using DHCP hi-jacking/DNS redirect functionality. This was removed in 7.0. You must change to using something like Comware portal redirect.
