---
author: lindsay
date: 2014-02-22 02:31:01+00:00
layout: post
slug: ccie-preparation-how-deep
title: 'CCIE Preparation: How Deep Do I Go?'
categories:
- CCIE
series: 'CCIE Preparation'
tags:
- CCIE
---

{% include series.html %}

CCIE candidates often want to know how deep they should go with each topic. There's an enormous amount of information to take in, and candidates are trying to find the balance between too much and too little. Here's how I approach it.

At some point on your CCIE journey, you will get sick of it. You’re investing a lot of time and energy in this, and sooner or later, you’ll reach a moment where you think “I just want to be over this, so I can move on with my life.” You need to be sure that you’re using your time in the most efficient manner possible. So you ask yourself: How deep should I go into topic _**X**_?

I could sum it up as:

> Deeper than any other certification you’ve done before, but don’t lose sight of the broader picture.

The question could also be looked at as “How do I approach topic _**Y**_?”

I’ve talked before about [study processes and scheduling]({% post_url 2013-11-26-ccie-preparation-study-scheduling %}), but what about figuring out how deep to go?


## General Approach


My general approach is this:


  * Read the [Expanded Blueprint](http://learningnetwork.cisco.com/docs/DOC-6864), and find all mentions of the topic. This is the ’source of truth’

  * Develop a base of ‘theoretical knowledge’, using classic texts such as [Routing TCP/IP Volume 1](http://www.ciscopress.com/store/routing-tcp-ip-volume-1-9781587052026). Then go to the Cisco [DocCD](https://www.cisco.com/cisco/web/psa/default.html). At this point, we’re trying to build up a conceptual model of how the protocol works.

  * For all sources of documentation, there’s two things to keep in mind:

    * Is this covered in the Expanded Blueprint?

    * Is this something that could be practically tested in the lab? What sort of questions could I be asked about this topic? What sort of configuration would they ask for, and what sort of troubleshooting might be needed if it breaks?



After doing your reading, you’ll do more hands-on study, often using vendor-developed training materials (e.g. INE, IPExpert). Whenever you think that you might be starting to go too far down a rabbit-hole, think about item 3 above again. Of course, when you think you’ve covered a topic, you need to circle back - have I learnt enough that I can answer anything they could be reasonably expected to ask on that topic?

I’ll use HSRP as an example of how to approach a topic, but first, let’s look at some general considerations for the exam.


## Remember: It’s a Configuration Lab


You must remember this:

{% include warning.html content="The CCIE practical exam is an 8-hour lab, covering configuration and troubleshooting on Cisco equipment. It is NOT a Design exam. It is NOT a multi-vendor exam. It is NOT a “Best Practices” exam. There are no servers to configure, only routers and switches. You are not going to be programming IOS, or writing your own implementation of the OSPF algorithm. All that matters is how to configure Cisco equipment to achieve a given result with specific protocols. You will be given a set of tasks, and all that really matters is the result: Does your network achieve the requested outcome?" %}


This is where I see candidates come unstuck. Yes, you must know how BGP works at a theoretical level. You must know how to configure BGP on a Cisco router to achieve a desired outcome. You must be able to perform troubleshooting as to why a network might not be working as expected. But you’ll be working within the constraints of IOS, using the configuration and debugging tools available to you. So stop worrying about the underlying code structures, and instead focus on the operation of the network, not the programming.

I’ve seen candidates say things [along the lines of](https://learningnetwork.cisco.com/thread/47181#259639) “Oh if I had Wireshark available in the lab, I could deconstruct the packets, and reverse-engineer the protocol, and it would be so much easier for me to pass the lab. I learn by looking at the packet structures.”

It’s fine to learn by looking at packet structures. But that’s not the key to passing this lab. You’re not designing a protocol. Cisco publishes plenty of documentation about how their devices operate, and how to configure them. That’s what matters here - you need to know how to configure the Cisco device to do what you want. Want to implement Dijkstra’s algorithm? That’s really cool, but this is the wrong exam for you.



## An example topic: HSRP



To keep this short, I’m going to use HSRP as an example topic. Topics like OSPF are much, much broader, but the concepts still apply - know your target and stay focused. Here’s how I would approach HSRP:



### Know your target: Expanded Blueprint References



The Expanded Blueprint only has a few mentions of HSRP:




    
  * HSRP

    
    * HSRP Between Two Routers

    
    * Pre-empt for HSRP

    
    * Authentication for HSRP






I’ve now got three things to figure out “How does HSRP work conceptually, how do I configure those items listed, and how might it break/how would I troubleshoot it?”



### The Documentation



HSRP is covered in [Routing TCP/IP Volume 2](http://books.google.co.nz/books/about/Routing_TCP_IP.html?id=GdWAapirZ9gC&redir_esc=y). Read that, and get a feel for the topic. The next place is the Cisco Configuration Guides. The “First Hop Redundancy Protocols Configuration Guide” contains a chapter on [Configuring HSRP](http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipapp_fhrp/configuration/12-4t/fhp-12-4t-book/fhp-hsrp.html). Definitely sounds like us, so start by reading this documentation.

The DocCD chapter looks pretty long though. How much of that do I need to know? Look at the list of topics. Is either foundational material, or relevant to features we need to know for the CCIE lab? Look through the contents of that chapter. Match them up against the Expanded Blueprint. It looks like there’s a reasonably close match. It’s really only the last few topics that don’t overlap (ISSU, SSO, BFD) - we can skip those.



### What might I be asked in lab?



So we’ve built up some theoretical, foundational knowledge. Now for where the rubber meets the road: What could I realistically be asked to do in the lab?

Firstly, configuration. These items look obvious:




    
  * Configure HSRP on two routers

    
  * HSRP authentication - text & MD5

    
  * Pre-emption

    
  * Configuring timers



It is entirely reasonable to be asked to consider any of those items. For any of them, we must know how to configure it, and quickly - preferably without looking up anything in the documentation. So that’s something we’ll practice doing until we know it cold.

What about troubleshooting? Ask yourself, what could go wrong with HSRP? The obvious thing is that the routers would not properly form an active/standby pair, and might both go Active. What could cause this?


    
  * Mismatched group IDs

    
  * Mismatched authentication

    
  * ACLs blocking HSRP packets

    
  * Broken connectivity between routers



Try injecting these sorts of faults into a test lab, and look at the results. How do you detect it? What commands do you need? (Show, debug, ping, etc). Learn those commands, and establish a method for isolating the fault. Know how to interpret the output of those commands, and know what result you’re looking for when running a command - ideally the result of each command pushes you down a different branch, getting closer to isolating the fault. Don’t just run commands blindly, hoping for some ray of sunshine.



### What Didn’t We Learn?



We've gone deeper than we had to for CCNP, and we can rattle off configs 10x faster than we used to. But did we learn a deep history behind the development of HSRP, or a philosophical discussion of HSRP vs VRRP? No. We will of course see the differences with VRRP once we study that section, but we're not going to get bogged down in it.

Did we learn how the Cisco developer implemented the protocol? Not exactly - we learnt how it operates, but we can ignore the underlying code. Did we spend hours analysing the contents of HSRP packets? No, we did not, because it’s not something that we’re going to be tested on. What we learnt was how the protocol worked, and how to configure & troubleshoot what we need for the lab.



## Stay Focused



You need to stay focused on your target. There are many that say that you should aim to be an expert first, and passing the lab will come as a by-product. There is a lot of truth to that, but here's the thing: If your goal is passing the lab, then you want to be an expert in CCIE Routing & Switching. Not an expert Developer. Not an expert in Security or ~Voice~ Collaboration. You can always dive much deeper into any specific area after you've passed the lab - but you need to be realistic about what you can achieve with the time available to you.

{% include note.html content="Parallels could of course be drawn with your professional work: You need to focus on what your business needs, and not let yourself get needlessly distracted or drawn down rabbit-holes." %}

