---
author: lindsay
date: 2017-07-10
layout: post
slug: everything-has-cost
title: 'Everything Has a Cost'
categories:
- Opinion
tags:
- Worklife
---

Everything comes at a cost: steak dinners & pre-sales engineering has to get paid for somehow. That should be obvious to most. Feature requests also come at a cost, both upfront, and ongoing. Those ongoing costs are not always understood.

It's easy to look at vendor gross margins, and assume that there is plenty of fat. But remember that Gross margin is just Revenue minus cost of goods sold. It's not profit. It doesn't include sales & marketing costs, or R&D costs. Those costs affect net income, which is 'real' income. Companies need to recoup those costs somehow if they want to make money. Gross margin alone doesn't pay the bills. 

## Four-Legged SalesDroids, and Steak Dinners

A "four-legged sales call" is when two people show up for sales calls. The usual pattern is an Account Manager for the 'relationship' stuff, with a Sales Engineer acting as truth police. These calls can be very useful. It's a good way to talk about the current business challenges, discuss product roadmaps, provide feedback on what's working, and what's not. The Sales Engineer can offer implementation advice, maybe help with some configuration issues. 

Often a sales call includes lunch or dinner. Breaking bread together is an important part of relationship-building. And everyone likes a free steak dinner, right? Of course, it's not quite free. 

Sending two people out to visit a customer is expensive. Travel time and costs add up. Providing 'free' advice takes time. Don't get me wrong: it is a very important part of the relationship. You want customers to have a good experience with your products. But those salaries need to be paid somehow.

Some sales calls are great two-way discussions, with both parties getting something from it. Users find out more about what's coming, and they have a chance to provide feedback to the vendors, to help make things better.

But other sales calls are a simple reading of a slide deck. Why waste your time, and your vendor's time if you just want them to give the standard pitch? 

It would have been far more efficient for everyone if people had just watched the YouTube video, or read the data sheet. Sending people out to do that doesn't make sense. There's a reason that many SaaS companies just publish a price list, and don't have big sales teams. There's too much overhead required.

## Features: Do You Really Need SNA, NVGRE, **and** FCoTR?

RFPs span the whole range from "_x_ ports at  _y_ speeds" to a complete laundry list of every networking protocol ever written. Sometimes they include RFPs that never made it out of draft, or features that no-one has ever implemented outside of a university lab. Sometimes they're ridiculously broad and claim that it is MUST HAVE for a desktop access switch to support OSPF, IS-IS, full-table BGP, SNA, AppleTalk, and IPX. Oh, and of course it should support every possible combination of optic ever produced, at every possible distance.

Plus the CLI: I want to be able to choose IOS, JunOS or Huawei-style CLI. Oh and don't forget the echo and chargen services. Very important. MUST HAVE.

Everything MUST HAVE when you know that perhaps 10% of the features will ever actually get used. 

But now the sales teams want a committed roadmap for when that core chassis switch with 288 x 100Gb ports will also support PoE+ on 10Mb thicknet. 

_Sigh_.

People just don't think it through when they ask for these features. 

Costs are not just incurred when developing features. They become a form of long-term technical debt. Every feature needs testing with every release. That feature needs testing with every other plausible combination of features. More features mean either much greater complexity in testing (and therefore longer test cycles), or worse: untested feature combinations.

If you are going to deploy a feature today, or in the near future, then by all means stipulate it. But don't put every possible feature in as MUST HAVE, if you're unlikely to ever implement it. And just because you've used [rlogin](https://en.wikipedia.org/wiki/Rlogin) since 1995, it doesn't mean that it's still a MUST HAVE requirement in 2017. Move on.

## Everything Has a Cost

For fun, take a look at the financial results for a company like [Cisco](https://investor.cisco.com/investor-relations/overview/default.aspx). For the quarter ended April 29, 2017, they had total revenue of $11.94B. COGS was $4.4B, for an overall gross margin of 63%. But then look at the Operating Expenses. R&D was $1.5B, while Sales and Marketing costs were $2.2B. Their net income was $2.5B, for a net margin of 21%. 

Yes, that's right: They spent 50% more on Sales than on R&D. All those sales calls and steak dinners add up. 

But pay attention to the overall numbers: Gross margin is just one financial measure. It is an important one, as it plays a large part in the marginal profits made from additional sales. But it's not the only one. R&D costs include the costs of feature development. It adds up.

> **NB**: I am not in any way picking on Cisco here. They are just the best-known company in the networking space, and their numbers are representative of the industry as a whole. You'll see similar numbers for other networking companies. Other industries will have slightly different percentages, but the patterns will be similar.

Pricing is much more complicated than taking input costs and adding a profit margin. But costs are still a factor. Higher costs mean higher prices. More time spent on sales calls and adding unused features raises costs, which has a good chance of raising prices.

To be clear: There's nothing wrong with spending time with a sales team. If you're planning a large purchase, you should spend the time to make sure you understand what is being proposed. Similarly, if you really need a feature, then by all means request it. But don't fool yourself into thinking that this comes for free. Everything has a cost.
