---
author: lindsay
date: 2013-07-01 20:30:51+00:00
layout: post
slug: imc-nnmi-which-one
title: IMC or NNMi - Which One is For Me?
categories:
- NMS
tags:
- hp
- imc
- nnmi
---

This is a companion post to my [review of the technical differences between NNMi and IMC.]({% post_url 2013-06-29-imc-vs-nnmi %}) That one tried to stay focused on technical matters, but this post is more opinion-based. It is intended to help potential customers decide which one is right for them. I am not going to directly compare them to the many other NMS products on the market - let’s keep this to a straight HP vs HP discussion.

{% include note.html content="Note: When I refer to NNMi below, I am referring to the overall NNMi suite, including iSPIS (for performance, NetFlow, etc) and NA (for configuration management). NNMi splits the various functions out into different applications, while IMC integrates more functionality into the base system." %}




## Target/Ideal Customer



The products come from very different backgrounds, and had very different target markets. But as IMC has rapidly added features, and the NNMi team have better integrated their individual components, the lines have gotten blurred.

**IMC:** The classic IMC customer that I see is an organisation that has decided to migrate from Cisco to HPN equipment. They’re looking for a network management tool that will deal with fault, performance and configuration, and will do it happily across Cisco and HP equipment. The friendly HPN salesman can do a good deal on IMC if you’re buying a lot of HPN equipment. Network sizes can vary, but will tend to be small-medium networks - let’s say up to 1,000 nodes. A typical IMC customer will be an engineer, not a manager. They will also be more inclined to configure it themselves, not using consultants. (I of course would recommend engaging consultants, at least for initial setup :D)

The new customer I see for IMC is organisations that are using HP wireless equipment, and want to implement a BYOD strategy. The future target/ideal customer will be organisations using HP’s SDN Controller, as that will heavily tie in to IMC.

Typical competitor products an IMC customer might look at: Solarwinds, Whatsup Gold, Cisco Prime LMS.

**NNMi:**
There’s three typical NNMi customers:




	
  1. Large, dynamic networks, with complex requirements. 20,000+ devices, with far more happening on the network than a regular admin can keep up with. They are drawn to NNMi for its scalability, advanced discovery capabilities, and policy-driven framework. With the right discovery, grouping and monitoring policies in place, NNMi can scale well across very large networks.

	
  2. Organisations have committed to the HP BSM suite. NNMi fits reasonably well into this, and ticks most boxes in an RFP. Note that often in these cases, the choice of NNMi is effectively forced down from management, and NNMi struggles for acceptance by the network engineers.

	
  3. People who were using the original OpenView back in the day dot, and have followed along with upgrades and migrations over the years. They probably still refer to it as OpenView, although that name was dropped several years ago. Mind you, no-one told the developers either, so we’ll let it pass.



One common point is that NNMi customers will be more inclined to use consultants to help with setup, configuration and ongoing maintenance. NNMi tends to be more “consultant-friendly” - this may or may not fit in with your organisation’s general culture.

Typical competitor products an NNMi customer might look at: CA Spectrum, IBM Tivoli, BMC Entuity.



## Key Points that Tip it Either Way



OK - we’ve seen the typical customers. But what if you’re somewhere in between? Maybe you’ve got 10,000 devices, but a low rate of change. Where would you fit in? Here’s some of the key areas that might tip your decision one way or the other:




	
  * BSM - if you’re committed to BSM, NNMi is the best choice, as NNMi will forward the necessary topology information to BSM.

	
  * Wireless - NNMi does not currently offer support for controller-based wireless networks. There are some workarounds, but you get very limited information, and it doesn't tie in with the rest of the Root Cause Analysis. IMC supports HP wireless controllers. If you are using HP wireless controllers, you should probably use IMC. If you are using autonomous APs, you could use either product.

	
  * BYOD - if you want to use HP’s BYOD strategy across your wired and wireless network, you will need IMC. You could use NNMi for monitoring, and IMC just for the BYOD component, but this creates a fair bit of overlap.

	
  * Integrations - NNMi (in one form or another) has been around a long time, and it has far more integrations supported out of the box. Many vendors will have documentation for integrating with NNMi. IMC currently doesn’t offer any standard integrations into other systems, although it can forward traps, or send emails. I expect this will change in time.

	
  * SDN - you want to start using OpenFlow across your network, and you’re going to use HP’s SDN controller. In this case, you must go with IMC.

	
  * RFPs - If you're in the habit of issuing very large, very detailed RFPs, NNMi will probably check more boxes. If you instead choose products by trying them out, IMC will look better. This is due to the different product philosophies - NNMi focuses on ticking off a list of capabilities, IMC focuses more on product look and feel, and usability. Both are valid approaches, but they are different target customers.





## Maybe I'm using the wrong product? Should I migrate?



Probably not. As a general rule, all management systems suck, in their own way. Ultimately the best system is the one that you pay attention to, and properly feed and water. If you just let the last system sit in the corner gathering dust, don’t expect a new tool to solve all your problems. That said, I can think of a few situations where you might move between NNMi and IMC (in either direction)




	
  1. You’re running NNM 7.50. That’s a legacy architecture, and moving to 10.00 is a massive change in approach, so it is worth doing a re-evaluation of your overall NMS approach, and not just upgrading. You should evaluate all alternatives, including non-HP ones.

	
  2. You’re currently running NNMi for fault and maybe performance monitoring, but you really need configuration management capabilities. You could add in licenses for HP NA (Network Automation), but the price scares you. If you’re not committed to BSM, and you’re purchasing some HP Networking equipment, you could probably move to IMC for less than the cost of your NNM support renewal.

	
  3. You’re using NNMi with the BSM suite, but it’s just not delivering for you, and you find yourself using only NNMi, and not the rest of BSM. If your organisation has had enough of BSM, and moves away, then it may be worth looking at IMC.

	
  4. You’re using IMC, but you want to tie into the broader BSM suite, to get greater correlation of events across all domains - network, servers, applications. If that’s where your company is going, you should look at migrating to NNMi. Be aware that full correlation of all events across all domains is not easy, and it may take a lot of time and effort to get the desired outcomes. It’s a noble goal, but not one that is easily achieved.

	
  5. You’re using IMC, and your network is getting very large, with a high rate of change. You’re finding that you’re doing too many manual tasks, and you need the ability to set policies to dynamically group objects and apply monitoring policies. If you’re large enough, the benefits of those policies can outweigh the overhead of defining and configuring them.





## What about running the two of them together?



Maybe I should use NNMi for discovery and fault monitoring, but IMC for configuration management for my HPN equipment? It's probably a lot cheaper than buying Network Automation licenses, right? As of NNMi 9.22, there is a supported integration between IMC and NNMi. In my testing, this did not work well. IMC can theoretically import nodes from NNMi, but it fails if you use DNS. NNMi can import nodes from IMC, and cross-launch from NNMi into IMC. Theoretically this works, but in reality you hit limitations - all nodes must be discovered in IMC, and added to NNMi via the integration - not discovered by NNMi. This is frustrating, since NNMi has a far better discovery engine. Also, all IMC nodes are added to NNMi as individual Custom Seeds - this will get impractical for large networks. Summary: You can do it, but don’t expect it to work well. Maybe in future.



## Conclusion:



Both systems are very capable, and they both have their place. Product choice will depend on your overall company strategy - if you’re committed to HP BSM, then choose NNMi. If your network is small-medium sized, IMC is a better choice for ease of use and base product functionality. If your network is very large and very dynamic, NNMi+iSPI+NA might work better due to its policy-driven functionality.

{% include note.html content="Last updated: 18th November 2014" %}

