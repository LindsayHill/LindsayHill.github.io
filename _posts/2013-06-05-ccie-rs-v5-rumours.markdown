---
author: lindsay
date: 2013-06-05 18:00:19+00:00
layout: post
slug: ccie-rs-v5-rumours
title: CCIE R&S v5 Rumours and Speculation
categories:
- CCIE
tags:
- CCIE
- cisco
- study
---

**Update: We're getting closer to an official announcement, and this post is attracting a lot of hits - you probably want to read my [latest post]({% post_url 2013-11-02-ccie-version-5-update %}), which contains more info.**

It's been over four years since the last change to the CCIE R&S blueprint. Version 4 was announced in April 2009, and went live in October 2009. Everyone thinks that the blueprint is due for an update, and soon. Here's my take on what changes might be coming up, and what I'd like to see.

Preparing for the CCIE lab takes a long time - 18 months is quite typical. Some take more, a few take less. Cisco publishes a "blueprint" of topics to study, and it's a very long list - something like 300+ lines in the extended version. Good candidates closely study this list, and make sure that they know every single topic on it, cold.

Because of the length of time involved, all candidates go through a phase of worrying "What if the blueprint changes?" People worry that they will get 80% of the way through, then find out there's a new list of topics to study. The longer it has been since a blueprint change, the more people worry.

I've been following a few forums, and watching a bit of the chatter. I've got some of my own opinions on what might change. Most of these are pure speculation, but first I'm going to let you in on a secret:

{% include note.html content="STP, OSPF, BGP, IPv6, Multicast will all be on the v5 blueprint. Same with at least 80% of what you're studying now. Core protocols don't change." %}

That's right folks, remember you read that here first. Now, on to the speculation:

## Timing

Hottest prediction: Announcement at [Cisco Live US](http://www.ciscolive.com/us/?zid=cl-global-hinav) on June 23-27 this year. If I was announcing it, I would probably do it at [TECCCIE-8000](https://www.ciscolive2013.com/connect/sessionDetail.ww?SESSION_ID=10733).

Cisco gives 6 months notice between announcing a new version, and it going live. It would probably get rounded up to a launch date of January 1 2014.

## Hardware

With version 4, Cisco introduced a virtualised environment, running IOU (IOS on Unix) for the troubleshooting section (TS). Using a virtualised environment gave Cisco tremendous flexibility to rapidly release updates, and just the sight of 30+ devices was enough to scare off those who'd only ever configured a small lab network.

The rumour for Version 5 is that they will also virtualise the Configuration part of the lab. People like Narbik ([Micronics Training](http://micronicstraining.com)) have been suggesting that the new [blueprint will have 10+ routers](https://learningnetwork.cisco.com/thread/49572#269652). The only realistic way to achieve this is with virtualisation. With the public release of products like the [CSR-1000V](http://www.cisco.com/en/US/products/ps12559/index.html), I see this as very likely. It gives Cisco enormous scope for reconfiguring the lab at will, delivering many lab variations.

The question then becomes: Do you fully virtualise the switching portion too? Or do you keep real hardware switches? I'm undecided on this. I suspect they would like to be able to virtualise everything, but I also think that they may want to keep some real switches. After all, they are still very much a hardware company (despite protestations to the contrary).

If Cisco does keep real switches in the lab, I'd love to see the [3850](http://www.cisco.com/en/US/products/ps12686/index.html) used. IOS-XE, MQC QoS, and plenty of spare processing capacity. This would be a forward-looking move, as the 3850 is clearly going to be Cisco's go-to switch in that segment for the next 5 years. I'm not convinced we will see it though, since it was only released earlier this year. I think the lab team needs time to thoroughly test kit/IOS, so they know that students won't hit bugs. (Aside: If you think you've found an IOS bug during your CCIE lab, you might want to double-check your config). I also suspect that the CCIE team doesn't get the budget to buy all new switches like that. More likely to use 3560-X switches, if they keep real switches.

You might also be asking: If Cisco virtualises everything, what does it mean for my home lab, or my rack rentals? If they've got a virtual platform, will they sell it to me? Either on a time-based access basis, or give me the software to run at home? Or will it leak? Yes, your lab of 1841s and 3560s will drop in value - but it's still not worthless. There will only be some parts of the blueprint that you can't study on older gear.

## IOS Versions

Currently Cisco uses 12.4(15)T on the routers, and 12.2(46)SE on the switches. These are very old - 12.4(15)T went End of Software Maintenance in [January 2012](http://www.cisco.com/en/US/prod/collateral/iosswrel/ps8802/ps6969/ps1835/prod_bulletin0900aecd801eda8a_ps6350_Products_Bulletin.html).

Using newer hardware (virtualised or not), I would expect to see 15.2 on routers, and 15.0 on switches (if they use them). This will clean up a lot of niggles and annoying limitations, and will mean that technologies like PfR are much more practical. Better IPv6 support across features too.

There will be a lot of problems for those using GNS3. Licensing and emulation will become very difficult. GNS3 users know this day has been coming for a while.

## Technology Changes

Rumours I've heard include:

* Frame Relay replaced with NHRP/DMVPN, but with no crypto.
* Simple point-to-point IPSec VPN.
* More PfR.
* IPv6 everywhere, including IPv6 Security.
* No more RIPv2.

I see the last two as most plausible. IPv6 should be completely integrated, not a separate section. The upcoming CCNA changes are dropping RIPv2, so it will probably be dropped here too. Cisco might say that having EIGRP is enough to cover distance vector protocols.

Frame Relay can feel like an anachronism, but it's useful for creating complicated network topologies that challenge your thinking about routing protocols. I would like to see it stay, but only for basic topology setup reasons - don't test any advanced features. One clue that it might not be going is that Frame Relay remains in the recently announced CCIE <del>Voice v4</del> Collaboration track.

Simple DMVPN and basic IPSec would make useful additions, given that CCIEs are highly likely to come across them, but do they need to be here? A good CCIE should be able to learn the basics of what they need to know. If they want more, there's always the Security track.

PfR is tough to love with 12.4(15)T, but it is apparently much more reliable, with many more features in IOS 15. Expect to see more of it.

## Question Styles/Exam Layout

I think the Troubleshooting section will stay. Candidates without strong Operational experience get very nervous about this section. I've seen many good engineers crack up under time pressure. It's like everything falls out of their head when they see a ticking clock. I think it helps to separate CCIEs from CCNPs.

If the Configuration Section does get virtualised, with many more devices, I think we'll see less basic setup, and more configuration of advanced features. So less configuring IP addresses, and more configuring/tweaking routing protocols and QoS, Multicast, PfR, etc. Some vendor labs in particular put a lot of time into configuring interfaces, but really, do you need a CCIE to type in 255.255.255.0 again and again? This sort of thing can make the exam harder though - if you build it yourself from the ground up, you have a better understanding than if you come in and work with a half-built network.

## What I'd Like to See

I would like to see some 'real' servers and clients added to the mix. DHCP clients, SNMP servers, syslog servers. traffic generating systems, etc. Some things are hard to simulate using routers and switches. With a virtualised lab, it would be possible to add small Linux VMs to use to simulate real-world traffic.

I would also like to see more EEM and TCL tasks. I think it's not enough for network engineers to just be fast at the CLI - I think you should also be able to do some basic scripting.

## What Should Candidates Do?

Firstly, don't panic. Even if there is a new blueprint announced next week, you'll still have 6 months to sit the lab. Seats will get tough to book. If you're well into your studies, book the lab as soon as you can, and hopefully you'll pass. If you're early on in your studies, make a note of the huge overlap that will exist, and just study those things for a while, until the vendors ([INE](http://www.ine.com), [IPExpert](http://www.ipexpert.com), etc) release updated materials. Note that the vendors are already thinking about likely changes coming up - they won't be taken by surprise.

{% include warning.html content="Don't Panic. Routing & Switching will still be Routing & Switching." %}

## But isn't this just all Speculation?

Yup. And you should take it all with a very large grain of salt. I'll be closely watching upcoming announcements at Cisco Live - let's see how far off the mark I am!
