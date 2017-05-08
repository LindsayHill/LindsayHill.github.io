---
author: lindsay
date: 2016-01-25 02:58:55+00:00
layout: post
slug: learning-to-love-codenames
title: Learning to Love Codenames
categories:
- Worklife
tags:
- worklife
---

One of the things I struggled with when starting at a vendor was dealing with project codenames. There is no [secret decoder ring](https://en.wikipedia.org/wiki/Secret_decoder_ring) - you have to learn the names the hard way. I couldn't understand why descriptive names weren't used. It took a while, but I've come to understand the reasoning behind the obscure names now. It's still a stretch to say I 'love' them, but I can at least understand them now.



## Naming Standards & Bikeshedding



When I started my professional career, it was common to name servers using things like Greek & Roman Gods, or Star Wars characters. Billing might run on Apollo, while Medusa was used for third-party connections.

This is fine for 5-10 servers, but clearly doesn't scale. I've wasted many long and pointless hours in server naming "[bikeshedding](https://en.wikipedia.org/wiki/Law_of_triviality)" discussions. Grumpy old sysadmins would argue that it was far easier to remember names like Bert & Ernie than web01/web02. The Young Turks saw that as a way of hoarding knowledge. It seemed to deliberately make it more difficult for newcomers/outsiders. They preferred descriptive names that gave some indication of what the system was doing, where it was located, etc.

Arguments went back and forth, then virtualisation came along and server numbers exploded. That killed the arguments for 'personalised' names.



## Project Naming



Project numbers haven't had the same explosion. It's still possible to have 'custom' names for each project. And so businesses do. These names don't describe the project functionality. Instead they might be planet names, animal names, cities, etc. There might be a vague theme - e.g. all versions of a software product might all be named after chemical elements. Other products will follow their own theme.

You might ask: Doesn't that have the same problems as server naming? Isn't it hard for new employees to understand what that project is? Why bother? Why not use something descriptive, like _Customer Billing v10.40_?



## There _Might_ be a Good Reason



I thought that secrecy must be the goal. "Let's use codenames like [Overlord](https://en.wikipedia.org/wiki/Operation_Overlord), [Manhattan](https://en.wikipedia.org/wiki/Manhattan_Project), [Torch](https://en.wikipedia.org/wiki/Operation_Torch) and play spies! No-one will know what we're up to. World domination will be ours!"

There is an element of this to it. Confidentiality does matter. But let's face it, it's just not that big a secret that a vendor is working on a new version of an existing product.

There is another pragmatic reason: The official product name can and will change, right up until just before product launch. If you give the project a name early on that sounds descriptive, then it starts to slip into the product. What happens if you change the name at the last minute?

For example, consider [Brocade Flow Optimizer](http://www.brocade.com/en/products-services/software-networking/sdn-controllers-applications/flow-optimizer.html). When Brocade was first working on that product, we could assign a working name of "Brocade Flow Twiddler." But then the developers will start working that into the product, and it will pop up in many odd locations. We might use commands like `btwiddle` .

Then a week before launch, the team realises that Twiddler is a silly name, and Optimizer sounds much more professional. Easy enough to update the marketing materials...but what about the code? Can we update all our code quickly and reliably? Maybe. Maybe not.

If we instead used the codename "Project Doohickie" then it's highly unlikely to end up in the code. The project team will know what Doohickie refers to, and there will be a little bit of secrecy. But it also makes sure that we can change the application name at any time, without risking leftover references.

I'm still struggling with recalling all the project names, or finding out how to look up the project name for an old product version. But at least now I think I understand part of why we use those codenames.

