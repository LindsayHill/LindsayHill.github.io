---
author: lindsay
date: 2013-05-07 07:50:09+00:00
layout: post
slug: hp-master-ase-hp0-y37
title: 'HP Master ASE: HP0-Y37'
categories:
- HP Master ASE
tags:
- hp
- mase
- study
---

I have just passed the HP exam "[HP0-Y37: Migrating and Troubleshooting Networks](http://www.certificationexplorer.com/Documents/HP0-Y37.pdf)." This means I I have now passed all required exams for "[HP Master ASE - Network Infrastructure 2011](https://www.hpe.com/certification/data_card/HP_MASE_Network_Infrastructure_2011.html)." This is a run-through of my impressions of the exam, and what I used to study for it.

## Overall Impressions:

This was a good, challenging exam. It made me think about the questions they were asking. Not just simple "How many gigawotsits in a flopsy?" questions, but instead questions that made me think, analyse the situation, and choose the right answer. Well, OK some of them you just had to know certain protocol rules, but they were the sort of thing you should have to know if you're working in this field.

The quality seemed good, apart from a few variations in diagram style. I didn't pick up any typos, and they have resolved the issue I had previously with 403 errors with some exhibits. I still don't like having to click up to four links for pop-up exhibit detail. Gets a bit crowded on the typical monitor seen in testing centres.

## Topics Covered/Exam Objectives:

HP publishes a detailed breakdown of the exam objectives:

* 7% - IRF - Intelligent Resilient Framework
* 14% - RRPP - Rapid Ring Protection Protocol
* 13% - BGP
* 5% - MPLS L3 VPNs
* 4% - MASE Networking Experience
* 18% - Migrating a Cisco Network to Open Standards
* 10% - Migrating Edge Devices
* 21% - Migrating and Expanding the Distribution Layer with HP Devices
* 8% - Migrating BGP

This is a pretty accurate list. When you're studying, go through that list, and ensure you understand what they're going to ask you. They're pretty clear about what they want. If you're like me, you might find some topics challenging - e.g. RRPP. I've never come across that in production networks, and to be honest, I'd be quite happy to never see it. It's not that bad, as far as protocols go, but I don't really need to learn a proprietary L2 loop prevention mechanism. I'd rather be learning TRILL or SPB.

## Resources used:

* HP [Practice Exam PRA-Y37](https://www.hpe.com/certification/practice_exams.html) - this is an online exam, with a similar format to the actual exam. The ordering process for this is very odd - you first buy a voucher, which varies in price depending on your country. The voucher takes a day or two to arrive (really? manual processing of an electronic voucher?). You can then use the voucher up to 5 times to "pay" for a web-based exam, taken through the PearsonVue website. The actual exam is quite useful, and I would recommend using it. The look/feel is very similar to the real thing. Each question has a breakdown of why a specific answer was chosen. Interface is a bit annoying at times though - you can't go back, and it doesn't tell you how far through you are. Cost $50USD for me.
* [HP MASE Network Infrastructure Official Certification Guide (Exam HP0-Y37)](https://www.hpe.com/product/HP+MASE+Network+Infrastructure+Official+Exam+Certification+Guide+Exam+HP0-Y37-Hardcover-6053). I used the Kindle version of this. This is a pretty good book, and covers most of what you need, but don't expect to pass on this alone. You need to do extra reading. If you've got a CCIE background, and/or you work with HP + Cisco kit regularly, you won't need too much more. This book includes a practice test. Note that the book contents don't line up exactly with the exam requirements - e.g. the book includes a section on MPLS L2VPNS, but these are not covered in the exam. My errata notes are [here](/assets/2013/03/HP0-Y32-Technical-Errata.rtf) (RTF). Cost me $70USD.
* HP Whitepapers on RRPP and IRF (Freely available, use Google).
* I created my own flashcards using [Mental Case](http://www.mentalcaseapp.com) as I went through various resources. I then set these to Long Term Repetition mode, and studied them as directed by Mental Case.
* ...plus lots of previous CCIE study

I didn't much in the way of specific practical learning for this exam. I have access to Cisco and ProCurve equipment, and periodically I get some access to Comware kit too. You don't **need** a lot of practical experience, but it will certainly help. You will want to have some Cisco experience for this exam. It's not a huge part of it, but when the exam deals with migrating from Cisco-proprietary technologies, you do need to understand those technologies, and their configuration.

## Administrative detail:

* Cost: $245NZD.
* Questions: 52
* Pass mark: 67%
* Time: 1 hour 45 mins

One important point to note about the exam is that it is presented in 10 "blocks" of questions. Each block has between 2 and 9 questions, generally focusing on one particular area. Within a block, you can move back and forth. But once you complete that block, you can't go back. You get told how many questions are in your current block, and which block number you're up to, but you don't know how many questions you've completed overall.I was keeping my own tally of completed questions - I recommend you do the same, so you can manage your time. I thought I was moving through at a reasonable pace, but I was starting to feel a little rushed towards the end, and I ended the exam with around 10 minutes to spare.

HP provides a very detailed score report - it's broken down into 21 sections, and tells you exactly how many questions were in each section, along with your score. So you can tell exactly which questions you got right/wrong. Don't ask me for my exact breakdown...I passed and that's all that matters!

## Was it worth it?

That's always the tricky one. I don't do hands-on networking all day - it's more of a peripheral thing to what I do. I won't directly get a pay rise for this, nor will I even get the costs covered by my employer. I did it more for the personal challenge, especially since I come across a lot of HP networking kit these days. If you've already completed CCIE, then it's fairly straightforward to achieve HP MASE. You only need to do two exams. If you don't hold any Cisco certifications, I think it might be as many as 5 exams to achieve HP MASE.

If you are a junior engineer, working with both Cisco and HP equipment, then I would recommend completing CCNA and CCNP first, simply due to wider market demand. But then I would cross over to the HP certifications, before moving on to CCIE. You'll need to put some time into it, but it's not overly taxing. Certainly not like the levels of effort you need for CCIE. You could also complete HP MASE before moving to CCIE.

If you've already achieved CCIE R&S, and you're looking for something a little different, I would recommend going to HP MASE. Your customers are probably going to be asking about HP networking gear soon, if they're not already, simply due to the price/quality point. You might as well get some experience with it now.
