---
author: lindsay
date: 2014-06-02 02:21:49+00:00
layout: post
slug: cisco-aci-messaging
title: Is Cisco Struggling with Their ACI Messaging?
categories:
- Opinion
tags:
- ACI
- cisco
- SDN
---

Cisco ACI represents a significant shift in the way we approach networking. This sort of shift will need massive customer education to explain their new vision. I'm getting the impression that Cisco is frustrated that their new messaging is not well-understood, and they feel that public perception is wrong, particularly in regards to NSX. Cisco seems to be blaming customers and analysts for this - should they instead be looking at themselves?

{% include note.html content="First some background: I don't work for a Cisco partner, so I don't get any special briefings. I don't have a '[rich nerd daddy](https://twitter.com/etherealmind/status/468130247062597633)' so I didn't attend Cisco Live. A quote for USD$300K+ of Cisco hardware is a very big order for us. I'm just a guy who's very interested in where networking is going, but all research is done on my own time. This places severe limits on how much I can dig into any one product set. I can't read every whitepaper. Instead I look at the higher level messaging, and I rely heavily on impressions from bloggers and analysts. Messages are diluted and filtered by the time they get to me. Same as it is for most customers." %}


## Attacking NSX


Keynote speeches during the recent [Cisco Live US](http://www.ciscolive.com/us/) conference went on the offensive against NSX, talking about scale limitations, dependencies/lock-in, etc. In itself, this was very interesting that Cisco dismissed all other players, and attacked NSX. Problem was, some of that information appeared to be at best disingenuous, at worst wrong. Ethan Banks has [written about some of this](http://ethancbanks.com/2014/05/27/vmware-nsx-multi-hypervisor-capability-and-fudslinging/):


> The takeaway is straightforward. Don’t get your information about a vendor’s product from a competitor. If you notice one vendor attacking another, assume there’s a really competent product being attacked – otherwise, why bother? Maintain a healthy skepticism of marketing claims, no matter where they come from. You MUST perform due diligence before bringing any solution into your network, especially one that will be disruptive.


I recommend reading Ethan's post, and the comments attached to it.


## Attacking Bloggers/Analysts/Customers


[Greg Ferro](http://etherealmind.com/) wrote an article about [ACI for Network Computing](http://www.networkcomputing.com/data-centers/cisco-faces-make-or-break-week-for-sdn/a/d-id/1252888?). The general thoughts gel with what I've heard, but Cisco didn't like it at all. Frank D'Agostino came out very strongly in the [comments section](http://www.bradreese.com/blog/5-26-2014.htm):

> For the amount of time Cisco has invested in you to help you understand what ACI is and isn't, you forever get it wrong
> Being technical is no excuse for puposefully misinforming people.
> This is pure BS, Greg has no idea on pricing, how open the platform is and the fact that we can not only existing on existing infrastructure but ACI can be extended over competitive infrastructure. Greg do your homework..
> This is a clear indication to me that the author knows nothing about taking a product to market.
> ...etc

What's interesting here is the way that the attacks are framed: "You're too stupid to understand ACI." Yet if they've invested a lot of time in helping Greg understand ACI, whose fault is it that the message is misunderstood? I also find it interesting to say "Greg has no idea on pricing" - so Cisco has invested a lot of time in explaining ACI to Greg, but they deliberately haven't mentioned pricing? Whose fault is that? Maybe they should be re-thinking their messaging.

Similarly, Twitter exchanges such as this are not uncommon:

> This tweet has been removed

i.e. the blame is on us for not understanding the message (because we're NSX fanboys apparently). Here's a hint: attacking potential customers for not understanding your product won't help close sales deals.

## Frustrations Boiling Over

My impression is that Cisco is letting their frustration show. They believe they have a developed a great set of products/technologies/architectures that can re-shape networking. They've been working on it for several years, and because they're deeply immersed in it, they can see the full scope of that vision.

There's just one problem: Too many people outside the Insieme Business Unit don't seem to get it yet. When you passionately believe in something, it can be very hard to understand why others don't immediately recognise its greatness. That is very frustrating, and I think it's boiling over into some of their messaging.

Technical people often make the mistake of assuming that if you don't understand how great my solution is, the problem must be with you: You're too stupid to understand my brilliance. But often it means that the solution has not been properly explained. Remember, you've got the full vision and background in your head - the other person doesn't.

I also suspect that Cisco's worried about NSX taking "mind-share" - it feels like there's a favourable general opinion towards NSX, and VMware's been pushing their message a bit longer. Cisco can see that there are limitations to NSX, and they feel they're not getting full credit for the completeness of their ACI vision.


## Aside: Pricing


There's been a lot of comments around [Cisco ACI/Nexus 9K pricing](http://herdingpackets.net/2013/11/13/cisco-application-centric-infrastructure-nexus-9000/), and in spite of what Cisco's said about price per port being market-leading, my gut feel was always that a complete ACI solution would be "[reassuringly expensive.](http://etherealmind.com/network-dictionary-reassuringly-expensive/)"

That makes it interesting to see Joe Onisick quote some [average street prices](http://www.bradreese.com/blog/5-26-2014.htm#comment-1415063068):

> The entry level Average Street price (ASP) we quote publicly is $125K US. That gets you the controller cluster, licensing, 2x 9396 leaf switches (48x full-rate 1/10G ports & 12x40G uplinks), and 2x 9336PQ spine switches (35x 40G line-rate.)


Those prices are very attractive relative to a quote I have in front of me for some 10G switches. I could throw away the ACI parts, and just use them as fast, cheap switches. The 9K software currently has a (deliberately) limited feature-set. But if you just want fast, cheap connectivity, that's tough to beat.


## It Will Come Right...eventually


I'm sure Cisco can get over this. They've got the resources, and plenty of very intelligent people. Once they get more products and code shipping, and a broad installed base doing interesting things, then wider acceptance and understanding will come. But they should be very careful about blaming customers for not understanding their vision. If the messaging is not being understood, then change the messaging. Don't vent your frustrations on us. We're your potential customers, remember?
