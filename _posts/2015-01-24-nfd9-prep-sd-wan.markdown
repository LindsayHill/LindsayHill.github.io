---
author: lindsay
date: 2015-01-24 02:55:47+00:00
layout: post
slug: nfd9-prep-sd-wan
title: 'NFD9 Prep: SD-WAN'
categories:
- Routing &amp; Switching
tags:
- NFD
- SDN
---

Software Defined WAN, or SD-WAN, looks to be a theme of [Network Field Day 9](http://techfieldday.com/events/nfd9), with presenters such as [CloudGenix](http://www.cloudgenix.com/) and [VeloCloud](http://www.velocloud.com/) showing us their offerings. At first glance, SD-WAN sounds pretty compelling. Who wouldn't want to slash their WAN OpEx? How do these solutions work, and do they have legs? I'm hoping to find out.

{% include note.html content="NB: I've lumped CloudGenix & VeloCloud together under the heading of SD-WAN. I'm not saying that they are the same though - I don't yet have enough information about them to fully understand the similarities and differences. I'm sure I'll know more in a couple of weeks!" %}




## What's SD-WAN all about?



{% include note.html content="**UPDATE 20150127:** [Greg Ferro](https://twitter.com/etherealmind) had written a piece on SD-WAN in his newsletter. This is also now available as a [blog post](http://etherealmind.com/2015-sdn-wan/) - I recommend reading it." %}


SD-WAN is about applying concepts of SDN to WAN networks. The goals are to increase flexibility and reduce WAN costs. This can be achieved through transport independence, dynamic path management, and better config management.

Historically we used private WAN circuits - leased lines, MPLS, etc. These had great SLAs, but the monthly costs were huge. The bandwidth was low, but guaranteed. Now that many places have access to high-speed Internet tails, it's a lot harder to justify that cost. It's very tempting to run IPSec VPNs across Internet links instead.

Those consumer circuits don't have SLAs, and make no guarantees about bandwidth. But still, if I could get a 100Mb/50Mb fibre link for ~$100/month, with LTE backup, surely that's going to be enough? If I only get 1/5th of the potential bandwidth, it's better than my 10Mb circuit at $2,000/month.

Traffic patterns have also shifted. Branch offices don't just do a few things to HQ - you now have users connecting to SaaS apps on the Internet, watching funny cat videos, and connecting to your company VPC at AWS.

SD-WAN solutions aim to simplify this, by making it simple to use whatever WAN connectivity is available at each site. Traffic flows should be able to adapt to  changing conditions. They form overlay networks that are transport-agnostic, and aim to hugely improve network management.

VeloCloud has quite a bit of info on its [website](http://www.velocloud.com/), while [Jordan Martin](https://twitter.com/BCJordo) noted that there's [not much info](http://www.jordanmartin.net/2015/01/17/nfd9-homework-cloudgenix/) out there about [CloudGenix](http://www.cloudgenix.com/) yet. I'm sure that will change.



## But Couldn't I Just...?



If you start talking to a network engineer about doing this sort of thing, they'll often say "What's so great about IPSec VPNs and failover paths? That's easy, I could do that right now with a few cheap routers."

To a point, they can. But that model can be difficult to scale, and it's not usually very dynamic. Simple configuration management can be painful. Routers might go to HQ first to get a base config applied, then shipped out. "Smart hands" might be needed to get it up and running at each site. Changing configurations to get branch offices to send some traffic direct to the cloud-based SaaS application is slow and manual. Simply dealing with the variety of possible connection types is a hassle.



## All About the Analytics?



As noted, CloudGenix doesn't have a lot of public info yet. But you can get hints about what a company is up to by looking at their [job ads](http://cloudgenix.jobscore.com/jobs/cloudgenix). An ad for a "[Network Data Mining & Analytics Engineer](http://www.jobscore.com/jobs2/cloudgenix/network-data-mining-analytics-engineer/aWPS5QSter47K1eJe9fLhG)" caught my eye. Key points include (emphasis mine):




    
  * Be the driving force behind **"mine the network” approach** instead of classical network stats and analytics....

    
  * 10+ years of relevant experience with a BS or MS degree related to data mining...

    
  * NoSQL and/or time-series optimized databases...

    
  * Exposure to 'R' or other similar languages for data mining algorithms.



I think that this is where the real value will come from. I can configure a network using IPSec VPNs today. But it's a static network, and my visibility is limited. SNMP only provides a subset of the information I need. NetFlow is typically only deployed at the main sites, and it is problematic to scale.

What I need is for the network to give me better application-level analytics, and be able to take action in response to changing conditions. It's easy enough to have a traditional network that can switch paths if a link fails. But it's hard to get a network to detect changing conditions, and redirect specific applications as needed.

If SD-WAN solutions can give me those sorts of capabilities, in a scalable, manageable way, I can see a huge number of use-cases.

Of course, I'm then left wondering: If I can do that on my WAN, can I also do it across my Campus & DC networks?



## What I'm Looking For



These are some of the questions I've got so far:




    
  * Integration/deployment - most companies have some form of existing WAN. How would I go about deploying an SD-WAN solution? Do I have to rip & replace everything, or can it integrate with existing networks?

    
  * How do the pricing models work? Am I paying per device, per month? In that case, what happens if I stop paying? If I decide I don't like Vendor <X>, am I stuck? (This isn't specific to SD-WAN. Migrating your current WAN solution is non-trivial.)

    
  * What are the hardware requirements & price-points? VM options?

    
  * Most SD-WAN models have some form of controller - can I run this in my own DC, or is it always managed by the vendor?

    
  * What happens when the controller is unreachable?

    
  * How can I extract data - e.g. application performance data?

    
  * Do they simplify connections to major cloud providers - e.g. [AWS VPC VPN](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Welcome.html) connections?



I'm sure I'll think of more before Network Field Day 9!

{% include note.html content="Network Field Day presenters indirectly cover my costs to attend NFD9, but I'm under no obligation to write anything. You can make your own assumptions about how biased that makes me." %}

