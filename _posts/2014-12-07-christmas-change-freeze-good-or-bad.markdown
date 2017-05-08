---
author: lindsay
date: 2014-12-07 05:50:44+00:00
layout: post
slug: christmas-change-freeze-good-or-bad
title: Christmas Change Freeze - Good or Bad?
categories:
- Worklife
tags:
- ITIL
---

We're approaching Christmas, and for many of us, that means we're about to enter an extended change freeze. This means an extended period when we shouldn't change anything, hoping to improve stability. ITIL Change Management tells us this is good. I'm not convinced.


## The Christmas Change Freeze


Many businesses impose some form of change freeze across all production systems during the Christmas/New Years period. In theory, all network/compute/storage changes are deferred until January. In practice, high priority changes will still be made if you jump up and down enough. The rate of change should still be lower during this period though.

Some change freezes may only run from just before Christmas until early January. Other businesses will go into a change freeze for as long as five weeks. My experience is that Southern Hemisphere businesses have a longer change freeze than Northern Hemisphere ones. I assume this is because many staff take extended leave over the Austral summer.

{% include note.html content="Aside: In New Zealand, the term ‘Brown out’ is often used when referring to the Christmas Change Freeze. I have no idea why this term is used, as a ‘brownout’ normally refers to something quite different." %}


## Why Have One?


There are differing opinions about the usefulness of a change freeze. [@Etherealmind](http://etherealmind.com/) thinks it’s a good excuse to put your feet up.

Dean Pemberton takes a quite different view, seeing it as critical for network stability:

> ... and most of you are about to enter brownout periods.
> 
> If you're not you should be, but that's another story, just let me know so I can change ISPs.

[NZNOG Mailing list, November 2014](http://list.waikato.ac.nz/pipermail/nznog/2014-November/021355.html)

Let’s drill into a few reasons for and against:


## The Case For


The thinking behind change freezes goes something like this:


  * Increased risk: Change is bad. If nothing changes, most stuff works without breaking. Changes might introduce buggy code, or someone will screw up the implementation. If we can stop change, we’ll lower our risk profile.

  * Increased time to resolution: Many engineers are on holiday during this time, so skilled staff may not be available to resolve issues

  * Increased impact: The network is particularly busy during this period, so any outages have a wide impact (this mainly applies to Telcos)

  * Conclusion: we should limit all change during this time window. (And if the ITIL zealots really get their way, all change would be banned forever).


## The Case Against


If you’re an ITIL believer, like most current Enterprise middle management, then the reasoning above is seductive. But is it correct, and is an extended change freeze the right response?

In the fifteen years I’ve been doing this, I’ve learnt this:


  * Change freezes are often in name only. Scratch the right backs, jump and down enough, and your changes get approved anyway. All you’ve done is added delay and extra hoops for engineers to jump through. No extra scrutiny of changes is done.

  * By imposing arbitrary dates, every project is under huge pressure to get their changes in before the freeze, to avoid a month-long project delay. This creates a period of massive stress on the implementation engineers in late November/early December, as they try to push through a high level of change. Tired people make poor decisions, and this leads to mistakes. Side effect: Change Management now becomes convinced that changes are the source of all problems, and next year wants to start the change freeze even earlier.

  * The same pile-up of changes occurs in the first couple of weeks post-change freeze, again leading to rushed implementations, and more mistakes.


By imposing change freezes, we only treat a symptom of the perceived problem. Rather than assuming change itself is bad, we need to look at our overall design, the product choices we’ve made, and how we’re deploying changes. If we are that scared of making change, shouldn’t that need re-thinking? And if we’re that worried about a few key people being away, doesn’t that represent a massive business risk?

Surely we should look at addressing the real problem here, not just covering up a few symptoms?


## Summary


I don’t believe that long change freezes make sense, for either customers or the business. It doesn’t improve stability, and it doesn’t reduce stress. Projects should be scheduled as normal, taking into account staff availability - so you're probably not going to be making changes on Christmas Day. Take real, useful action about why your changes are risky, and manage your key staff risks better.

But if your company does have a long change freeze, learn how to take advantage of it. Use it as a lever to get agreement for time off for a proper holiday. Or watch the clever older engineers, who know that it’s a good excuse for doing nothing for a few weeks, and getting paid for it. Then you can use your annual leave some other time, when everyone else is doing real work. It also means you can go to a few more of those vendor booze-ups.
