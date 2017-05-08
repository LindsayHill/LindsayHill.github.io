---
author: lindsay
date: 2015-02-19 19:30:32+00:00
layout: post
slug: nfd9-cumulus-networks
title: 'NFD9: Cumulus Networks'
categories:
- Routing &amp; Switching
tags:
- NFD
- SDN
---

Cumulus Networks gave a great presentation at [Network Field Day 9](http://techfieldday.com/event/nfd9). They presented their vision of how they’re working to improve networking. But they were also clear about what they don’t do, and where they will instead enable others.


## Linux on a switch? Seems natural to me


Many network engineers started out running cables, and doing low-level networking. They build up to designing & running more complex networks. I came at it from a different direction. I first ran Linux systems in 1999. My first professional job was working with HP-UX in 2000, and I later moved into running Check Point firewalls on [Nokia IPSO](https://en.wikipedia.org/wiki/Nokia_IPSO). I was well-used to working with Unix-like systems, and it was completely natural to me to run tcpdump on a network device.

To become an effective network security engineer, I had to learn more about routing & switching. But because I had that \*nix background, I was always frustrated by the limited capabilities offered by IOS. `| include` is a poor substitute for `grep`. Yes, you can do some stuff with TCL, but would you want to? Packet capture was a poor joke until recently.

So when I first heard about Cumulus, it made a lot of sense to me. Run Linux on a switch? Install custom packages to do whatever I wanted? Sign me up!


## Network Disaggregation


The more important implication of what Cumulus is doing is that it separates the hardware from the software. Pick your hardware, and then run your choice of software on it. Exactly like we've been doing with servers since we started moved away from Big Iron Unix to Linux/Microsoft.

This has significant implications, because the cost of software development can't be 'hidden.' The hardware and software components are clear. This has the potential to dramatically shift the price discussions - even if you don't use Cumulus.

They're already shaking up the market - look at what's happening with [Dell](http://www.dell.com/learn/us/en/uscorp1/secure/2014-01-28-dell-open-networking-cumulus), [Juniper](http://www.networkworld.com/article/2855056/sdn/juniper-unbundles-switch-hardware-software.html) and now [HP](http://www.networkworld.com/article/2884208/sdn/hp-latest-to-unbundle-switch-hardware-software.html). Vendors are changing their models, and we can all benefit from that.


## A Turtle-Powered Rocket? Wait, maybe I misinterpreted that :D


The Cumulus Mascot is the “Rocket Turtle” - a Turtle with a Rocket strapped to its back.

[![cumulus_turtle](/assets/2015/02/cumulus_turtle-e1424373714458.png)](/assets/2015/02/cumulus_turtle.png)

cf [RFC1925](https://tools.ietf.org/html/rfc1925):


> “With sufficient thrust, pigs fly just fine.”


Cumulus have assured me there is no particular symbolism to their mascot:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/northlandboy">@northlandboy</a> <a href="https://twitter.com/wyrdgirl">@wyrdgirl</a> no particular symbolism, it was just meant to look nice :)</p>&mdash; CumulusNetworks (@CumulusNetworks) <a href="https://twitter.com/CumulusNetworks/status/567397485245521920">February 16, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I still see it as a metaphor for what Cumulus is doing. Sometimes it feels like the network is slow-moving, but there’s a good reason for that: Everyone blames the network. The result is that we become cautious. Vendors take time to roll out new features, and new protocol adoption is slow. We want to be sure that everything works together, and doesn’t break existing services.

But what if we want to make a change to our network? What if we want to do something that systems aren’t designed for today? We put in our feature request, and maybe if we’re big enough, the vendor thinks about adding it.

What if I could change the code for the BGP daemon myself? Or write a Bash or Python script to make my device do something different?

That’s where Cumulus is trying to strap a rocket to a turtle. They’re giving you the tools to change the network faster.


## A Different Operational Model


The flexibility offered comes at a price though. With traditional network devices, all configuration is in one place, through one consistent interface. Cumulus doesn’t work like that at all. It’s like a Linux-based server, where configurations are spread around different files. It is possible to make changes to the running state of some services, without it appearing in any configuration file.

If you’re used to running Unix-like systems, this is all pretty straightforward. But if you’re used to working with Cisco devices, it’s a big change. Simple things like backups are more complicated - there’s not just one file, there’s a collection.

This is manageable. You shouldn’t be configuring all your devices manually anyway. But our operational models will have to change. Luckily is no shortage of good resources on managing Linux systems. Most of that will apply to Cumulus Networks. But it still won’t be the right model for everyone.

I highly recommend watching [the videos](http://techfieldday.com/appearance/cumulus-networks-presents-at-networking-field-day-9/), to better understand what Cumulus is up to. In particular, I recommend this video where Dinesh Dutt talks about network configuration simplification:

<iframe width="560" height="315" src="https://www.youtube.com/embed/B7aaX2wItPg?ecver=1" frameborder="0" allowfullscreen></iframe>

{% include note.html content="Aside: The attitude of the Cumulus Networks staff impressed me. They sought out opinions on what they’re doing, and where they should go. Some companies are too sure of themselves, and believe they’ve discovered the One True Way. If only." %}


The grumpy Unix beardy-type in me loves what Cumulus is up to. I don’t know how it’s going to work out, but I do think that their approach and attitude can change our industry. I recommend trying out their [demo workbench](http://cumulusnetworks.com/get-started/test-drive-open-networking-in-our-remote-lab/), to see if it works for you.

{% include warning.html content="Disclaimer: As a paying presenter, Cumulus Networks indirectly covered my costs to attend [NFD9](http://techfieldday.com/event/nfd9). But I am under no obligation to write anything. My opinions are my own." %}
