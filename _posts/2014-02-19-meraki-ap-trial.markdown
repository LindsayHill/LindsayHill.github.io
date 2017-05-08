---
author: lindsay
date: 2014-02-19 19:00:37+00:00
layout: post
slug: meraki-ap-trial
title: Meraki AP Trial
categories:
- Routing &amp; Switching
tags:
- meraki
- wireless
---

[Cisco Meraki](http://meraki.cisco.com/) offers a [free wireless AP](https://meraki.cisco.com/lp/free-demo) to anyone who registers for a webinar about their products. I had given up on receiving my AP, but after a moan on Twitter, it arrived. I've been using it in the office for a few weeks for simple wireless access, and I'm pretty impressed by it. The management interface is pretty slick, and it would clearly solve a lot of problems for anyone dealing with a widely distributed network.


## The Meraki Point of Difference

Meraki sells wireless Access Points, switches and security appliances. On their own, they are perfectly reasonable devices. But Meraki's Unique Selling Point is that all their systems are "Cloud-Managed". Rather than individually configuring devices via console/SSH, all configuration is done through an AWS-hosted Dashboard. The devices automatically phone home periodically, and will pull down any configuration changes. That means that you don't need to do any pre-configuration of devices before shipping them out. Instead you can get them shipped directly to the remote site, and you just add the device ID to your dashboard. Tell the local staff to plug in the right ports, and the device will automatically get a DHCP IP, and pull down its configuration. You set all the policies and config centrally.

Meraki refers to the management as "[Out of Band](https://meraki.cisco.com/trust/#oob)" - user data flows as per normal, but management/configuration information goes via the central system:

[![](https://meraki.cisco.com/img/trust/data_flow.png)](https://meraki.cisco.com/img/trust/data_flow.png)

{:.image-caption}
Meraki Data Flow - "OOB" Management

No user data goes via Meraki, and the devices continue to operate if the dashboard is offline. It's only configuration & central monitoring that is unavailable - but you can still locally monitor the devices. You just can't change their configuration if the Meraki dashboard is unavailable.

You're not just paying for the device - you are paying a regular subscription for the management platform too. They used to have an online calculator, but they seem to have taken this down - must be Cisco policy that you have to talk to salesperson? You pay an upfront fee, plus annual licensing. If you stop paying your license fees, the device will stop working.

Meraki was acquired by Cisco in 2012. So far they seem to keep doing their own thing, but I'm unclear on the future. I expect that the hardware will change, but the cloud-based management platform will remain - that's where the real value lies.



### Radio Performance - A Quick Note



I'm not a wireless network engineer, and I'm not qualified to speak in depth about the radio performance of the AP, nor would it be fair to Meraki - the trial AP is a single-radio, 2.4GHz-only system. Their full range includes dual-radio and 802.11ac-capable gear. I can't imagine many people are still deploying single-radio gear these days. Makes sense to use these older/slower units for a free trial though - after all, you want to get customers exposed to the management UI, which is the real differentiator. The wireless performance was fine for what I was doing, but I was much more interested in the management platform.



## General Impressions



Initial setup was very easy. I just had to set the right mode for my AP (bridge mode), set the SSID + desired security, and I was up and running. Pretty soon I was looking at graphs showing clients, application traffic, etc:

[![Meraki Applications](/assets/2014/02/meraki_apps.png)](/assets/2014/02/meraki_apps.png)

I could see my clients, their names, OS types, Wireless adapter manufacturers, applications, etc. I quickly found a couple of misconfigured clients that were using the wrong AP. It was a massive jump in visibility from the previous "dumb" AP I had.

There's a lot of interesting features that I've only just started digging into:




    
  * Packet Capture, with integrated upload to [CloudShark](http://cloudshark.org/).

    
  * Observed SSIDs

    
  * Presence Heatmap showing observed client traffic patterns/locations over a time period.

    
  * L7 Firewall and traffic shaping - e.g. control access to specific sites, or rate-limit iOS updates.

    
  * Access policies - set Splash pages, require customers to purchase access, etc.

    
  * "Make a Wish" box on each page - any time you think of something you'd like to improve, there's a little box on each page, where you can put in a quick note about how you think it might be improved.



Pretty much everything I can think of that I'd want to do with a wireless AP, and it all seems reasonably straightforward to configure. I'm impressed - except for IPv6, I wasn't impressed that "IPv6 forwarding is disabled by default for performance reasons."



## Management Integration



Network Management is a large part of what I do, so I'm keenly interested in how you could integrate Meraki-managed systems with the rest of your network management systems. It's cool having a fancy dashboard for your wireless gear, but I don't want another stand-alone tool. I want to be able to get fault, configuration and performance data into other systems - e.g. to automatically log a ticket in Service Now when an AP goes offline.

SNMPv2 & v3 access is available to either the APs directly, or to the dashboard. This is quite nice - I can either directly discover & poll APs, or I can just monitor the Dashboard, and automatically add new devices. I think it would be reasonably straightforward to configure something like ScienceLogic to connect to the Dashboard, discover all APs, automatically create virtual devices for each AP, and then display status & performance data, per-AP. Integrating with the dashboard simplifies the problems of having to manually add devices to your NMS.

They also have an [XML API](https://kb.meraki.com/knowledge_base/meraki-xml-api) that can let me expose AP names, status, location, etc. The problem is that I can't restrict access to that data - I can either enable or disable API access, but then I can't apply any access controls to it. Many organisations will therefore leave API access disabled. I also haven't seen anything that lets me automatically configure and provision systems - e.g. by connecting to their API to make changes that would be pushed out to the managed systems.

Devices can be set to send syslogs, but it is network-wide - i.e. I can't have different devices sending syslogs to different syslog servers. Probably not that big a deal - I can break up my "Organization" into multiple "Networks", so I could have different policies if I needed to.

Meraki also offers email alerting for specific conditions - e.g. a device going offline. I'd like to see a few more options here about triggering remote API calls, but email gives us a few options at least.



## Would I Use/Recommend Them?



I don't have a specific need for a wider Meraki deployment right now. But if I was dealing with the challenges of managing a very distributed network, particularly one made up of many small offices and shops, I would certainly have them on my list to evaluate. They go a long way towards solving most of those problems, and doing it well. It saves a lot of time being able to do simple things like ship gear direct to the remote site, without having to pre-configure it first.

What about using it for Wired network gear? For small sites, yes, I would certainly consider adding low-end access switches to Meraki management, to tie in nicely with the APs. I would only really want to do it for sites that have users only, and no local services. I don't think I'm quite ready to move to a fully cloud-based management model for core switching though. I think I'd want to retain the ability to locally configure and manage systems. That opinion may change in the next couple of years. I'm interested in watching what Cisco does with them.

{% include note.html content="Disclaimer: Cisco Meraki shipped me a free AP, as part of the standard program they offer everyone. They did not ask me to write about my experience." %}

