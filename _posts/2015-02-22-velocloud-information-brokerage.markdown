---
author: lindsay
date: 2015-02-22 03:48:15+00:00
layout: post
slug: velocloud-information-brokerage
title: VeloCloud & Information Brokerage
categories:
- Routing &amp; Switching
tags:
- NFD
- SDN
---

[VeloCloud](http://www.velocloud.com/) was the first presenter at [Network Field Day 9](http://techfieldday.com/event/nfd9). They are one of the new breed of SD-WAN vendors. I'm impressed by what they're doing, and and the potential it offers for re-thinking the way we do WAN connectivity. But I think the most interesting part is the increased visibility into how networks are performing.

I won't go into the details of how it all works - [Brandon](https://twitter.com/SDNGeek) covers some of it [here](https://ccie31104.wordpress.com/2015/02/16/velocloud-impressions-from-nfd9/), and you can look through [VeloCloud's site](http://www.velocloud.com/products/features/) to understand it more. I want to focus on a few details around data analysis, and information brokerage.


## Internet Quality Monitoring


In this video, Kangwarn Chinthammit talks about how VeloCloud is using their devices to monitor Internet quality. Because they're installed in a wide range of locations, with many different WAN connection types, they're building up some interesting data.

<iframe width="560" height="315" src="https://www.youtube.com/embed/oEGJZuX3a5k?ecver=1" frameborder="0" allowfullscreen></iframe>

They've been able to do some deeper analysis of the data, and break down quality measurements by location, circuit type, hour, and day. Some of the interesting results include:

    
  * A good ISP in one location may not be any good in another. So you can't just pick one ISP.

  * Quality varies during the day, and across the year. It might be getting worse, then be good for several months, before it gets worse again.

  * Higher bandwidth links may not deliver better quality.


The upshot of all that is that you need multiple links, and you need to be able to move traffic between them, as conditions change. Of course, VeloCloud also thinks they've got the solution to do just that :)


## Two So-So Links == One Good One?


VeloCloud has measurements that indicate that Internet links are unsuitable for real-time application delivery up to 25% of the time. If all we do is dynamically redirect traffic between two links, does that mean that we can get down to 6.25% poor performance?

VeloCloud says that if you combine the dynamic redirection with their on-demand Forward Error Correction, you get that down to < 1%. Watch the demonstration video to see the impact:

<iframe width="560" height="315" src="https://www.youtube.com/embed/mdNbNn4Ucy4?ecver=1" frameborder="0" allowfullscreen></iframe>

The use of the cloud gateway is what allows them to apply some more smarts to traffic redirection, FEC, and packet reordering. It's a pretty slick demo, although I have to admit to getting distracted by the action sports they were showing...


## Information Brokerage


It's the data collection aspects that interest me the most. Firstly, if I can collect detailed application performance data, I can use that in my other monitoring systems. This helps with understanding what's happening, and alerting on fault conditions.

As an end-user, I could also use this data to analyse which of my ISPs were working well, and which were having problems. I could use it as stick to beat the ISPs with. If I'm paying consumer-grade pricing, there's not much I can do. But if I've got business-grade circuits, or private WAN connectivity, I've got some good data to go to my ISP with. I'm particularly interested in seeing if the measured performance of those 'business' circuits really is any different. Or am I just paying extra for the privilege of being able to speak to someone on the phone when I ring the ISP?

VeloCloud is in a more interesting position though. They're collecting wide-scale data on ISP performance across time & geographical location. This information has real value to ISPs. They can use it to help figure out where to invest, and it could be used in competitive analysis.

That second part is a bit tricky - VeloCloud has to be very careful about who they share data with, and in what form. They might be able to sell detailed performance data to an ISP, if it's for that ISP's own circuits. But if they become known for selling that same data to competitor ISPs, they'll breach that trust, and lose that revenue stream. They can publish aggregate/anonymised data, but it gets tricky with detailed data.

It also has implications for who they might take investment capital from. What if Verizon wanted to invest in VeloCloud? Would they then lose their value to other ISPs?

You can see this play out in other fields too - e.g. [Tail-F](http://www.tail-f.com) worked with many networking vendors, and were seen as independent. But then [Cisco acquired](http://newsroom.cisco.com/release/1438152) them. This acquisition makes Tail-F worth less, because their customers will need to re-think their approach, and probably stop using them. But it's a great strategic move by Cisco, because it makes their competitors divert resources. Cisco has recently made some parts of Tail-F [freely available](http://www.tail-f.com/cisco-accelerates-greater-adoption-of-network/), but that's just further distraction for their competitors. Well played.

I like what VeloCloud is doing in the SD-WAN space. Cheaper & better connectivity with greater visibility - what's not to like? I look forward to seeing how all the players in this space execute over the next few years. I also look forward to seeing just what we can do with this wider view of how networks are performing.

{% include note.html content="Disclaimer: VeloCloud was a paid presenter at [NFD9](http://techfieldday.com/event/nfd9). This means that they indirectly covered my costs to attend. I am not paid or obligated to write anything, and my opinions remain my own. " %}

