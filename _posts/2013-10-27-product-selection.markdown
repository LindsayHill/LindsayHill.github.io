---
author: lindsay
date: 2013-10-27 17:00:35+00:00
layout: post
slug: product-selection
title: 'Product Selection: It''s Not Always The Best Technology'
categories:
- Opinion
tags:
- sales
---

Many engineers assume that product selection is as simple as finding the product with the best combination of features that still comes in under budget. Should be easy, right? Err...so why did the boss over-rule me again, and insist on buying another Check Point firewall? Turns out there's more to purchasing than just finding the best product. Here's a few of the non-technical items that factor into it.

## Overall Direction

Managers hate orphaned products. They hate the look of something that just doesn't quite fit - e.g. the single Solaris system, when you're a Windows shop, or the single Juniper router, when all the rest of your network is Cisco. So sometimes you'll choose something that's "Good Enough", because it fits into your overall direction. Aside: I think this is why Cisco ASAs are widely deployed - they're not that great, but they're Good Enough, and they fit into the overall strategy. Managers also like the idea of "one throat to choke", although I've never actually seen that play out particularly well. Large companies are quite happy to point the finger internally - e.g. I can't get HP to fix the NNMi/IMC integration, because it's two different arms of a very large company.

## Supportability - Skills + Vendor Support

It's important to be able to easily access skills for supporting your product choice. HP-UX might be 5% faster for your application, but if Linux is good enough, then it's probably a better choice: I can easily find Linux admins (yeah OK, that was a contrived example!). Similar with OSPF vs IS-IS: IS-IS is technically superior, but for most applications, OSPF is probably a better choice, simply because I can easily find engineers who have worked on OSPF networks.

This ties into why management may prefer to go for something off the shelf, vs developing a custom solution. They can then get support from a vendor, rather than relying on a couple of pony-tails in the corner. You can probably write a fantastic application, but management thinks: What if I need to sack you? Of course, sometimes the only option is to write it yourself - but know when it makes sense to do it yourself, vs using a pre-written product.

## Market Acceptance

> No-one ever got fired for buying IBM/Cisco/Microsoft

I was involved in a purchasing decision recently where an explicit requirement was that the product chosen must come from one of 4 large vendors. It didn't matter that there were alternatives that were technically far superior, at a lower cost: For this specific need, it was critical that the product chosen be seen as coming from a large, well-known, well-accepted vendor. Variations on this theme also occur where a company has a short-list of approved vendors. Buy off that list, you're fine. Try to go off the list, and you'll find it's just not worth the pain. Sometimes there are good reasons for these policies, sometimes it's just to make it easier to justify your decision should it all go horribly wrong.

## Company/Product Viability

This item relates to the previous one. Let's say I'm purchasing a new NMS. This is not a set-and-forget system. It's something I'm going to invest in for years, and changing will be painful and time-consuming. So I need to know that the company and product will be around for at least the next 5 years. I want to know that product development will continue, and that I will get the ongoing support I need. Choosing a product from a startup represents a high risk: Will they pivot to something else? Or run out of cash? Or (worst of all) get purchased by CA, and doomed to a long slow death by negligence?

## Accept What You Can't Change

Frequent disagreement on purchasing decisions will eventually lead to a form of dissonance, and you will wonder if you should stay with this company. Ask yourself why you disagree with the decisions that were made. Calmly talk it through with your boss, and try to understand the reasoning behind the decision. Were there other factors you haven't considered? Or is your boss getting huge vendor kickbacks, and is now completely one-eyed? If the latter, then you need to decide if this is something you can just accept, or if you need to find another job. Endlessly fighting it will just wear you down. If you believe another product is far better, and you believe your company has made a serious mistake, it might be worth looking for a new job. If it's just one of those things, and you can accept the alternative, then just let it go.

Whole books have been written about purchasing decisions, and I'm not sure I'll ever fully understand them. Just remember, next time the boss buys something you don't like, ask yourself: "Am I seeing the whole picture here?"
