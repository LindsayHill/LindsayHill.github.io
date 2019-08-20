---
author: lindsay
date: 2018-06-16 17:06:41+00:00
layout: post
slug: long-support-lifecycles
title: Don't Rely on Long Support Lifecycles
categories:
- Opinion
- Worklife
tags:
- worklife
---

I hate long support lifecycles for hardware and software. Yes, you should be able to buy a new iPhone or switch and use it for 3+ years. But some people want 10+yrs of support, and wail and moan when vendors end support. This is wrong. It drives up costs & complexity, and makes your systems **less** robust, not more. It's a false sense of security. Plan to buy smaller & cheaper, and upgrade frequently.

## Why Vendors Don't Like Them

Vendors don't want to do long support lifecycles. They will do them, because people pay for it, but there comes a point where they put a line in the sand. "Sorry, that system is now EoL."

Why?

* **Costs:** Testing software and hardware combinations is hard work. Add many years of released hardware & software combinations, and it gets _much_ harder. More racks of gear & more permutations == more costs.

* **Complexity:** It's hard enough to test against a small set. But now you have to deal with obscure systems acquired from a third party 7 years ago? Complexity == time and money.

* **Motivation:** Hands up who wants to work on legacy systems? Exactly. It's hard to motivate engineers to support older systems. People want to work on new & interesting problems. So you end up with junior engineers supporting the old stuff.

* **Hardware:** There's two angles here: one is that it is hard to deploy new software features on older hardware. By committing to supporting old hardware, you constrain development of new features. The other is that hardware needs to be over-engineered when it first ships, raising the cost. Think about your chassis switches: How much extra capacity needs to be built in now, to allow you to plug in new line cards in 5 years? How much do you think it costs to add that capacity now, well ahead of when it will be used, and right at the limits of engineering capacity?

## Why They're Also Not Good for Customers

It's not just vendors. Long support lifecycles are not good for customers either, often because they create a false sense of security.

### "Forgotten" Systems Catch You in the End

Many years ago my team managed several messaging services. The vast majority of customers were on well-managed, modern systems. But for various reasons we still had a handful of services running via some legacy systems.

> Aside: this is completely normal: **Every** non-trivial business has some legacy systems.

The old boxes sat there, chugging away. Everything worked, and no-one ever touched them. At one point they had an uptime of over 1,000 days. In my naive junior engineer days I thought that high uptimes were a Good Thing. They were still under support. All was well, right?

Not quite. We had a power failure at that site, and a server did not come back up.

Problem was, it had been so long since anyone did anything to them that no-one remembered how they worked. The hardware was old, the OS two generations behind, and the documentation non-existent.

We had vendor support involved, but they too were a bit hazy on some of the details, since the OS & hardware was so old. It took several days to cobble together a working system, and get services up again.

We should never have let it get that bad. We knew the systems were old, we knew we needed to move away, but "Well, it's still under support...so we'll put that migration off another year."

### Sometimes You're Fooling Yourself

In the above example, we had vendor support engaged, and they were able to help, although it took a bit longer due to the age. But other times you find that the vendor either cannot, or will not, properly support old systems. They are very happy to take your support fees. But when you log a case, their first response is always "Upgrade to the very latest version and see if the problem re-occurs." Yes, I'm looking at you here, Check Point.

Chances of getting a code fix, or even a decent investigation into an issue are near-zero.

Better to have been carrying out regular upgrades, staying current.

## Does it Ever Make Sense?

Maybe. There are scenarios where you have a system that you know is going to be hard to upgrade/replace, **and** you know that requirements won't change. For example, a router deployed to a very remote data collection site. You still want to have a plan to be able to replace that device. But your plan might mean knowing what the "new" system you'll use is, rather than stockpiling spares.

Think about your overall service health and robustness. What's going to make a system more robust? One that never changes, or one that is regularly tested, regularly exercised, with staff that **do** know what all systems are doing?

If a system doesn't change for a long period of time, you lose institutional knowledge on how to deal with it. Then when you **are** forced to change, you don't know what to do. Even long support lifecycles come to an end . Better that you prepare for it.
