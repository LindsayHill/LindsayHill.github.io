---
author: lindsay
date: 2014-02-06 02:25:19+00:00
layout: post
slug: sciencelogic-architecture-overview
title: ScienceLogic Architecture Overview
categories:
- ScienceLogic
tags:
- sciencelogic
---

This is a basic overview of the [ScienceLogic EM7](http://www.sciencelogic.com/) system architecture, describing the various components, their functions, and how they can be combined or split across multiple systems. I’ve been deploying ScienceLogic systems recently, so this blog acts as my own notes/summary.

The ScienceLogic system can be broken down into five functions:

1. **Database:** Stores all configuration and performance data. The heart of the system.
2. **User Interface:** Web interface, where users interact with ScienceLogic.
3. **Data Collection:** Scheduled polling of monitoring systems, using SNMP, WMI, XML API, etc.
4. **Message Collection:** Receive inbound SNMP traps and syslog messages.
5. **API:** External systems can interact with ScienceLogic API, either to retrieve data, or to submit events and change configuration - e.g. Add new devices, change monitoring policies, etc.

Where this gets interesting is that these functions can be deployed on one centralised servers, or spread out across multiple physical and virtual servers. You can run everything on one system, like this:

[![SL All-In-One](/assets/2014/02/SL-All-In-One.png)](/assets/2014/02/SL-All-In-One.png)

Or you could fully separate the functions across multiple systems:

[![SL Fully Distributed](/assets/2014/02/SL-Fully-Distributed.png)](/assets/2014/02/SL-Fully-Distributed.png)

Note that there used to be a separate "Integration Server" for providing API functions. As of recent versions, this is no longer a dedicated system type - instead you can use an Administration Portal or Database Server.

You could have some combination of the above components - e.g. Combine message & data collection on one system, and UI/DB & API on another:

[![SL Mixed Mode](/assets/2014/02/SL-Mixed-Mode.png)](/assets/2014/02/SL-Mixed-Mode.png)

Splitting up configuration/data storage from collection lets you spread out your monitoring load, and/or work around challenges with network topologies, overlapping address space, etc:

[![SL Multitenant](/assets/2014/02/SL-Multitenant.png)](/assets/2014/02/SL-Multitenant.png)

All systems can be either physical or virtual, and all run the same code base. Splitting it across boxes does not significantly increase your administration overhead either. Patches are uploaded to the central server, and then deployed to all systems in one go. In theory, once you’ve set everything up, you shouldn’t need to manage any individual systems.

Check the "ScienceLogic Architecture" manual for more information about the architecture: [HTML version](https://portal.sciencelogic.com/files/documentation/7_3/architecture/sciencelogic_architecture.htm), [PDF version](https://portal.sciencelogic.com/files/sciencelogic_architecture_7-3-5.pdf) (Registration required; may need to be partner or customer. Yes it sucks that the documentation is behind a reg-wall).

The ability to break out systems, and spread load across multiple collectors provides us with more capabilities around HA, DR and scaling. I’ll take a look at those in a future blog post.
