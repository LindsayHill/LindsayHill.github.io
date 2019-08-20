---
author: lindsay
date: 2014-12-13 21:30:54+00:00
layout: post
slug: rate-my-ios
title: Rate my IOS?
categories:
- Routing &amp; Switching
tags:
- app store
- cisco
---

Review schemes are useful for identifying good consumer products and applications. But that doesn't mean that everything needs to prompt me to leave a review. Cisco has started prompting for reviews for IOS versions, but I'm not convinced it makes sense for network operating systems. Perhaps it will do one day when disaggregated hardware/software is the norm for network devices.

## Reviews for Consumer Apps - no problem

I love the [1Password password manager](https://agilebits.com/onepassword). It's a well-polished app, and has been great value. Part of making my life better means not annoying me with frequent prompts for review:

> 1Password never prompts you for a review. We value your workflow too much to interrupt it. If you feel generous and have a couple of minutes, please leave a review. It means the world to us.

I like the [Pocket](http://getpocket.com/) app too. It prompts me to leave a review every single time it gets updated, which annoys the hell out of me. But hey, it's free, so maybe I shouldn't complain too much.

Pocket and 1Password are examples of consumer applications in a competitive market. The barrier to switching is relatively low, and they live and die on reviews. In a crowded market, customers rely on reviews, and the App Store review schemes work well. But that doesn't mean that every piece of Enterprise software needs a review system.

## Consumer Reviews for IOS?

I recently download a Cisco IOS image for a colleague to test out the [Flow-Based SPAN](http://www.cisco.com/c/en/us/support/docs/lan-switching/switched-port-analyzer-span/116133-maintain-flow-span-00.html) feature (aside: the docs say it will work on a 2960-S, but it doesn't). A week or so later I received an email from Cisco, asking me to rate that IOS version:

> Please tell us about your experience with the software. Your opinion matters to us and other customers. Take a minute to submit a review.

Following the [link](https://tools.cisco.com/Cisco/rnr/RatingWidget/readReview.htm?appCode=SDSP&pageNo=1&ratings=true&productId=282867573&entityType=release&Param2=15.2.2E1&entityId=15.2.2E1282867573&Reference=eMail&Param1=Catalyst%202960S-48TS-L%20Switch), I'm prompted to leave a 1-5 star rating, along with Pros and Cons. There's only one review there currently - a 1-star review:

> When using Port-Channel function for uplink, the port will disconnect frequently.

Hmm, not encouraging. It doesn't replace a support function either - the response from Cisco is basically "What's the SR number?"  But maybe it saves Cisco from doing some testing, by highlighting problematic releases.

## Where's the value?

There's two problems here. One is that Cisco will never get a sufficiently balanced range of reviews to provide any meaningful information to admins trying to decide which IOS to run. They will get a handful of 1-star reviews from people who've struck bugs, and maybe a couple of 5-star reviews. Most people will never bother leaving a review, because they don't feel strongly enough about it. If you have millions of users, you might get hundreds of reviews, and you can form opinions from that. But IOS images just don't have that many users. Perhaps they need to make it compulsory for all members of the [Cisco Champions](https://communities.cisco.com/groups/cisco-champions) to leave a review, to get some more data points?

The second problem is that I don't have any meaningful software choice. If there are a lot of bad reviews for a password manager, then I can look at alternatives. What do I do if all the reviews for IOS are bad? I can't run anything else on Cisco hardware. I have a limited amount of choice amongst versions, but it usually comes down to 1 or 2 versions.

Maybe it will make more sense in future, when we have a disaggregated model, where hardware is separated from software. We're moving towards that. Right now I can buy network switches that can run a choice of Big Cloud Fabric, Cumulus, Dell Network OS, Pica8, etc. Soon [Junos will be available](http://www.networkworld.com/article/2855056/sdn/juniper-unbundles-switch-hardware-software.html) as a choice. Will IOS join the list of options? What would that look like, if we had a full range of choices, and extensive reviews & ratings available for each?

I can dream I guess.
