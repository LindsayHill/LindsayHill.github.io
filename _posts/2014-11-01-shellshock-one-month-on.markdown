---
author: lindsay
date: 2014-11-01 08:18:30+00:00
layout: post
slug: shellshock-one-month-on
title: 'Shellshock: One Month On'
categories:
- Security
tags:
- ISIG
- presentation
- security
---

[Shellshock](https://en.wikipedia.org/wiki/Shellshock_(software_bug)) was released a little over a month ago, to wide predictions of doom & gloom. But somehow the Internet survived, and we lurch on towards the next crisis. I recently gave a talk about Shellshock, the fallout, and some thoughts on wider implications and the future. The talk wasn't recorded, so here's a summary of what was discussed.

### Background: NZ ISIG: Keeping it Local

The New Zealand Information Security Interest Group (ISIG) runs monthly meetings in Auckland and Wellington. They're open to all, and are fairly informal affairs. There's usually a presentation, with a wide-ranging discussion about security topics of the day. No, we don't normally discuss 'picking padlocks, debating whose beard or ponytail is better or which martial art/fitness program is cooler.'

Attend enough meetings, and sooner or later you'll be called upon to present. I was 'volunteered' to speak on [Shellshock](https://en.wikipedia.org/wiki/Shellshock_(software_bug)), about a month after the exploit was made public. I didn't talk about the technical aspects of the exploit itself - instead I explored some of the wider implications, and industry trends. I felt the talk went well, mainly because it wasn't just me talking: everyone got involved and contributed to the discussion. It would be a bit meaningless to just give you the slides. Instead I've written up the talk into a blog post format, and tried to incorporate some of the points that came up during the talk.

1. [Shellshock: Bigger than Heartbleed?](#bigger-heartbleed)
2. [Was it all over-blown?](#all-over-blown)
3. [A new class of attack?](#new-class-attack)
4. [Loss of Diversity vs Centralised Control](#loss-diversity)
5. [What's Next?](#whats-next)
6. [Have big OSS users been getting a free ride?](#oss-free-ride)
7. [It will happen again: What can we do?](#will-happen-again)
8. [Closing thoughts](#closing-thoughts)

## Shellshock: Bigger than Heartbleed?   {#bigger-heartbleed}

As is the current style, Shellshock was released to great fanfare, with a catchy name, and Doomsday-style headlines. A representative sample:

[El Reg](http://www.theregister.co.uk/2014/09/24/bash_shell_vuln/):

> Patch Bash NOW: 'Shellshock' bug blasts OS X, Linux systems wide open

[BBC](http://www.bbc.com/news/technology-29361794):

> Shellshock: 'Deadly serious' new vulnerability found

[New Zealand Herald](http://www.nzherald.co.nz/business/news/article.cfm?c_id=3&objectid=11331892):

> Shellshock: Bash bug 'bigger than Heartbleed'

Even this from [CNN Money](http://money.cnn.com/video/technology/2014/09/29/bash-bug-shellshock.cnnmoney/):

> 'Shellshock' can hack lights in your house

Yes, really.

We had all the ingredients for a right doozy of a bug: widespread deployment of vulnerable software, easy to exploit, high visibility. We saw statistics coming out of [Akamai](http://www.akamai.com/html/security/through-the-bashdoor.html), [CloudFlare](http://blog.cloudflare.com/inside-shellshock/), [Incapsula](http://www.incapsula.com/blog/shellshock-bash-vulnerability-aftermath.html), etc showing a quick ramp-up in attack traffic, from many sources.

We then started seeing reports of [compromised systems](http://www.stuff.co.nz/technology/digital-living/61733004/yahoo-hacked-using-shellshock-flaw):

> Yahoo hacked using Shellshock flaw

It looked like it was going to be carnage. Wide-spread attacks, worms, major meltdowns, the ingredients were there...and yet it didn't quite pan out that way. We're still here, the Internet still works, and it turns out that the Yahoo compromise was [just another bug](http://www.computerworld.co.nz/article/556771/yahoo-says-attackers-looking-shellshock-found-different-bug/):

> Yahoo says attackers looking for Shellshock found a different bug

## Was it all over-blown?   {#all-over-blown}

So was all the hype over-done? Yes, it probably was. It was certainly a nasty bug, but it had several factors that limited its impact:

* It was relatively easy to filter via IPS/ADC
* It was reasonably simple to patch, without requiring any service restarts
* Bash isn't actually all that widely used on custom appliances - they tend to use more limited shells
* Applying a simple code patch is a lot easier than fixing application logic flaws

But perhaps the hype also helped. If bugs are only known as something forgettable like [CVE-2002-0649](http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2002-0649), it doesn't get any attention. Give it a memorable name and a logo, and suddenly the public starts talking about it. Maybe it's like Hurricanes/Cyclones? They all have names, and people take them seriously. We can connect to "Hurricane Lucy" far better than we could to "Tropical Storm 2014-10-482." Perhaps we need to have a policy of naming major exploits, with the first of each year starting with A, the second with B, etc?

This hype helped spread the message, and encouraged people to take action to defend themselves. It's hard getting approval to patch an obscure bug, but if your management is reading about it in the paper, they want to know you're doing something about it.

## A new class of attack?   {#new-class-attack}

There was some talk of this being a 'new class of attack.' But it's not really - I see it as just another [Injection Attack](https://www.owasp.org/index.php/Top_10_2013-A1-Injection). The OWASP Project defines this as:

> [Injection flaws](http://www.owasp.org/index.php/Injection_Flaws) occur when an application sends untrusted data to an interpreter

Yep, sounds like what we've got here. The same old story: Don't blindly trust what users tell you. Sanitise it first.

The good news here is that it's pretty easy to patch a code flaw like this. Fixing SQLi logic flaws across many applications is a lot tougher.

The bad news is that Bash is probably going to be rich source of further exploits, now that some light has been thrown upon 25-year old code.

## Loss of Diversity vs Centralised Control   {#loss-diversity}

The main commercially available banana today is the [Cavendish](http://en.wikipedia.org/wiki/Cavendish_banana_subgroup). Plants are not just the same type - they are genetic clones. This gives consistent quality, and we can finely tune our growing processes. But this comes at a risk: A single disease could wipe out the entire industry. That's pretty much what happened with the earlier [Gros Michel](http://en.wikipedia.org/wiki/Gros_Michel_banana).

I'm grappling with this in the context of software. Historically we had a wide variety of custom software, but now we prefer to use pre-written components and libraries. There's no point in everyone trying to write their own TCP/IP stack - it's better to focus on areas of differentiation. Look at how network gear is changing: We're moving from [custom code](http://en.wikipedia.org/wiki/Cisco_IOS), to [blended systems](http://www.cisco.com/c/en/us/products/ios-nx-os-software/ios-xe/index.html), and on to [almost completely open switch operating systems](http://cumulusnetworks.com/).

But what does this mean? We end up with a lot of shared code, and hopefully that's better quality code. Fewer crappy pieces of code, where developers have tried to create their encryption systems. But when that shared code has issues, it affects almost everything. An exploit in an underlying library or utility could leave almost all our systems vulnerable.

We're also seeing a concentration of services on a few large cloud providers. If they have issues (security or otherwise), it has a wide impact.

Consider [CloudFlare](http://www.cloudflare.com/) as an example. A non-trivial amount of Internet traffic is routed via CloudFlare systems. If we find an exploit that affects their systems, it could lead to large-scale compromise. But the flip-side of that is that they can ensure that attacks are mitigated where possible, and secure configurations are the default. Look at their recent response to POODLE: they disabled [SSLv3 by default](http://blog.cloudflare.com/sslv3-support-disabled-by-default-due-to-vulnerability/). It takes a lot of time & effort to make those changes across many different systems - but because they act as a central choke point, they could quickly make the change.

What's better: Diversity or Control? No easy answer here, and maybe it depends on your scale.

## What's Next?   {#whats-next}

I had been going to say that SSL would be the next one to get hit...and then [POODLE](http://en.wikipedia.org/wiki/POODLE) came along.

People used to attack Windows, because it was easy. But when that got tightened up, the attacks moved to the applications. They then moved to the plugins (Flash, Java, etc). People have been banging on obvious daemons like OpenSSH and Apache for years. Most of the issues have been worked out there. Now the attacks are moving to the libraries and other utilities called by those systems.

It needs to be something in widespread use, and something that probably hasn't had enough eyes looking at it. That means probably something Open Source again. Sorry.

My pick: Compression or image handling libraries. Who's up for a sweepstake?

## Have big OSS users been getting a free ride?   {#oss-free-ride}

We know the line about "Many eyes make all bugs shallow," but we also know that a lot of code simply doesn't have many eyes looking at it. Open Source is wonderful, but it's impractical for most businesses to do a line-by-line review of all the code they depend upon: there's just too much of it.

What about the big players in the market, that have built enormous businesses on top of Open Source Software? Shouldn't they be returning something to the community, by putting in resources where others can't?

Turns out that some of them are. Heartbleed was discovered by [Neel Mehta](http://en.wikipedia.org/wiki/Heartbleed), doing a line-by-line review of OpenSSL. Google's [Project Zero](http://googleprojectzero.blogspot.co.nz/) has given some of the smartest hackers free rein to find & fix bugs wherever they can, even if it's not software Google has developed or is even using. No doubt they will come up with some more major bugs that will cause panic, but they should improve the overall health of the ecosystem.

Facebook also gives back at [code.facebook.com](https://code.facebook.com/). It's not security-specific, but they are clearly contributing back to the Open Source communities they are built on.

But Amazon? So far as I can tell, Amazon doesn't feel it's necessary to contribute back. I guess the freedom of Open Source Software also includes the freedom to **not** contribute.

## It will happen again: What can we do?   {#will-happen-again}

We can't completely stop these things happening again. We can hope to A) reduce the chances of it affecting us, B) limit the potential damage, and C) responding to events.

There's a variety of techniques for trying to minimise risk (configuration, limit attack surface, etc) and limiting damage (containerisation, control inter-system communications, etc).

But the area I want to look at is response. I've seen several articles talking about how we need to ensure our responders aren't burnt out from dealing with POODLE, Heartbleed, Shellshock, etc. The problem is that there's so much manual effort being expended. I'm still seeing people doing this:

```bash
lhill$ scp bash-4.1.2-15.el6_5.2.x86_64.rpm lhill@server:
bash-4.1.2-15.el6_5.2.x86_64.rpm    100% 905KB  85.0KB/s   00:00
lhill$ ssh server
Last login: Wed Oct 15 14:31:50 2014 from 192.168.225.1
[lhill@server ~]$ sudo rpm -Fvh bash-4.1.2-15.el6_5.2.x86_64.rpm
Preparing...                ############################ [100%]
   1:bash                   ############################ [100%]
[lhill@server ~]$ logout
```

This has to stop. We have to stop [Feeding the Machine with Human Blood](https://www.usenix.org/sites/default/files/conference/protected-files/underwood.pdf). If you're still manually rolling out patches, you have to look at your systems for managing change. You have to figure out how you can quickly and automatically test & deploy changes. This has massive extra benefits: It's not just security.

## Closing thoughts   {#closing-thoughts}

Shellshock wasn't as bad as it could have been, for a variety of reasons - some technical, some marketing. It was just another widespread vulnerability, but it looks like the Internet is still standing.

These things will happen again, and probably sooner rather than later. It is pleasing to see that Good™ people are actually looking at these things though. It's not just the bad guys. But that means that we will see more public exploit disclosures, and we will need to reconfigure hundreds/thousands of systems quickly. Look at your systems for managing the rapid rollout of change - this has much wider benefits than just security.
