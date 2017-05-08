---
author: lindsay
date: 2015-02-01 23:01:54+00:00
layout: post
slug: ccie-renewed-exam-400-101
title: CCIE Renewed - Exam 400-101
categories:
- CCIE
tags:
- CCIE
---

The problem with obtaining certifications is that you need to renew them. CCIE is no different - I first [passed the lab]({% post_url 2013-02-17-ccie-success %}) in September 2012, and I was overdue for renewing it. I'm pleased to report that I have now done that, and it is now current until September 2016. Here's some of my impressions of the 400-101 exam.

I had planned on using the [CCDE written exam](http://www.cisco.com/web/learning/exams/list/ccde.html) to renew my R&S CCIE, and then decide if I would go on to attempt the CCDE practical exam. But it seems that the CCDE exam writers and I just don't share the same mindset. I tried, but it wasn't working for me, and I wasn't making progress. So I went back to R&S for my re-cert.


## New Blueprint


I originally passed version 4, exam number 350-101. This has been updated to version 5. The written exam is now 400-101. Of course, this doesn't mean that everything changes. Core L2 & L3 protocols don't change that much. BGP, OSPF and EIGRP and still BGP, OSPF and EIGRP.

There are some key changes though, such as:


  * Frame relay gone, DMVPN in

  * IS-IS back in - theory only

  * New EIGRP features, such as named mode

  * More MPLS - e.g. VPLS

  * Basic IPSec

  * More IPv6 coverage


Sadly my pet peeve of IPv6 tunnelling techniques is still there. I'm sorry Cisco, but I just don't care about tunnelling IPv6. Native all the way! Still too many references to classful networks too. Textbook writers love it, but no-one's used that in production for 20 years.


## Preparation


Because there's a lot of overlap with the earlier version, and what I've studied before, I could target my study. My primary preparation tool was the [Cisco Press](http://www.ciscopress.com/) books "CCIE Routing & Switching v5.0 Official Cert Guide." Cisco keeps expanding the CCIE blueprint, so what was one very heavy book has been split into [Volume 1](http://www.ciscopress.com/store/ccie-routing-and-switching-v5.0-official-cert-guide-9781587143960) and [Volume 2](http://www.ciscopress.com/store/ccie-routing-and-switching-v5.0-official-cert-guide-9781587144912). Thankfully I was using [Safari Books](http://www.safaribooksonline.com/) on my iPad, so I didn't have to lug them around.

There's also been a change of author - Wendell Odom has passed the baton over to the one & only [Narbik Kocharians](http://www.micronicstraining.com/).

My preparation was to read through the books, and update my [CCIE flashcards](http://www.mentalcaseapp.com/) as I went. I still have my set from previous study, and just needed to update them to include new topics. Those were the ones I focused on more. I didn't spend a lot of time labbing up topics - I've done enough of that in the past. I only labbed things I didn't properly understand, or where I was pretty sure the textbooks were wrong. (Aside: I always feel like I'm making progress with a topic once I start seeing errors in books/documentation).

People that are new to CCIE-level study think that they can just read the Official Cert Guide, and that will give them enough information. That works at CCNA & CCNP level, but CCIE candidates know better. This time around was no different. There is so much to cover in the textbooks that they just can't do it all. Whenever you come across a topic that you're not comfortable with, you need to find other sources of information. My primary data source for this is the Cisco documentation, and occasionally clarification from other blogs for very specific points.


## Exam Impressions


The exam was about what you would expect - some easy stuff, some tricky stuff, and some stuff that I just had no idea about.

Some questions were great - they tested your understanding of concepts. Others were subtle - they approached a topic from a different direction, testing whether you actually knew what was going on.

A classic technique used in CCIE-level exams is to test whether you know **all** the ways to solve a problem, not just the '**best**' way. My general approach with multi-choice is to ignore the answers at first, and try to solve the problem. Then I see which answer matches mine. This helps avoid getting distracted by other answers that seem plausible. But Cisco can be tricky. So I'll 'solve' the problem, but find that my answer is not listed as an option. Often this means that I've mis-read the question somehow. But you might dig deeper, and realise that they're deliberately doing it, to see if you know the other ways of solving the problem. Sneaky.

As noted above, the blueprint has changed. I'm not giving away any state secrets by telling you that if it's on the blueprint, it is fair game to be tested. Make sure you use the blueprint for your studies.

That said, there were a few questions where I really didn't know the topic they were referring to. Looking at the blueprint later, those topics seemed a bit of a stretch from the blueprint. But maybe that's why Cisco says [this](https://learningnetwork.cisco.com/docs/DOC-22705):


> The following topics are general guidelines for the content likely to be included on the exam. However, other related topics may also appear on any specific delivery of the exam.


Could also be that those were some of the 'testing' questions, where they don't count towards your final marks. But overall I felt it was a 'fair' exam.


## Where to from here?


My CCIE is now current until September 2016. Because of the way the expiry dates work, I could sit this same exam in another 6 months, to push that date out until September 2018. But I'll probably leave it at least 18 months, because, well, life.

In the short term I need to update my VCP-DV, and then consider my next moves. I don't think that will involve any more certifications. It will be more about studying new technologies, trying out products like [Brocade Vyatta Controller](http://www.brocade.com/forms/jsp/vyatta-controller/index.jsp?src=SEM&lsd=GGL&lst=Display&cn=SDN-GDG-15Q1-EVAL-Vyatta-Controller&cid=lp_networkdestiny2_so_ggl_00001), and writing about them. Plus I've got this [little event called NFD9](http://techfieldday.com/events/nfd9) coming up next week that will overload my brain...
