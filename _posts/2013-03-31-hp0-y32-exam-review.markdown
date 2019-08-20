---
author: lindsay
date: 2013-03-31 00:04:12+00:00
layout: post
slug: hp0-y32-exam-review
title: HP0-Y32 Exam Review
categories:
- HP Master ASE
tags:
- exam
- hp
- mase
- study
---

I have to pass two exams to complete [HP Master ASE - Network Infrastructure]({% post_url 2013-03-21-hp-master-ase-network-infrastructure %}). I recently passed the first of these, "[HP0-Y32: Designing and Troubleshooting Open Standard Networks](http://www.certificationexplorer.com/Documents/HP0-Y32.pdf)."

## Preparation

I did not attend any official training. I read the official [HP Press Study Guide]({% post_url 2013-03-25-hp0-y32-study-guide-review-and-errata %}) for this exam. I spent around 3-4 weeks reading this in my spare time. As I went through each chapter, I made flashcards covering items that seemed important to me. Depending on your background, you'll need to pay more attention to specific areas - e.g. you may need to pay extra attention to Comware configuration if you haven't used it before.

I set the flashcards in [Mental Case](http://www.mentalcaseapp.com/) to "Long Term Study" mode. Every day, I would flip through whichever cards Mental Case said were due.

Each chapter had practice questions that I went through after reading the chapter. The only other preparation was completing the Sample Test of 74 questions at the end of book.

I didn't worry too much about any other specific labs or practice. I completed CCIE R&S last year, so most routing/switching concepts were fairly fresh. I did do some small labs with a ProCurve platform I have access to. I have managed Comware-based platforms in the past, so I'm generally familiar with their configuration. This is not a test of speed configuration (like the CCIE lab), so I didn't feel the need for large amounts of hands-on time. The ideal setup for this lab would be to have two of each of Cisco, Comware and ProVision switches, so you could try out all kinds of weird and wonderful interoperability scenarios.

{% include warning.html content="I didn't find any other useful blogs out there about this exam. Sadly, if you plug the exam code into your favourite search engine, 99% of the results are from cheating sites. Hopefully this blog will put at least a bit of legitimate info out there." %}

I also read several free PDFs from HP, covering IRF Configuration, ACL & QoS Configuration, and PVST+/MSTP interoperability.

## Topics/Coverage

HP is pretty explicit about what the exam covers. The exam blueprint says that the questions are split like this:

* 44% - Troubleshooting HP Networks - covers Layers 1-4.
* 49% - Interoperability - VLAN management, Link Aggregation, STP/MSTP/PVST+, IRF, Layer 3 Redundancy, OSPF and NAT
* 7% - HP Network Design Principles

I can assure you that the exam was pretty much just as advertised. At first it felt like I was getting hammered on Interoperability questions, but then it rounded out with more troubleshooting.

The troubleshooting section is a little harder to study for, as most of this comes from experience. If you understand the underlying protocols, and have experience working in real networks, you will be fine. It's not going to go incredibly deep into a specific protocol, but you should be able to look at a problem description and symptoms, and be able to make an educated guess about where the problem might lie.

{% include note.html content="As an example, consider a problem description that shows an interface in down/down state: The answer is not going to be an ACL issue! (Not a question on the real exam, of course)" %}

Interoperability is probably easier to study for. They tell you what areas to focus on, and it's not that big a list. E.g. STP - you need to know what the capabilities are of each platform, and what it means when Cisco switches run PVST+, and your HP switches don't talk PVST+. You need to understand what will happen when you introduce standards-based equipment into your "Cisco-enhanced" standards-based network. There's a limited number of possible combinations here, so you should make sure you understand what they are, and how different vendors behave. Understanding the underlying protocols is key here. If you understand how STP actually operates, you should be able to work through any possible combination. The challenge I have with STP is that sometimes I'm a bit slow working through the process. Know the rules, and you can work through any topology.

Don't expect to pass this exam with just Comware/ProVision knowledge - at least one question was entirely Cisco-based! A bit odd for an HP exam, but acceptable for what they are trying to test with this exam. Don't worry though, it was not a deep technical Cisco question. Anyone with experience of mixed platforms would be fine.

## Difficulty

This was not an easy exam. It was not over the top in difficulty, but I found that many of the questions needed a bit of reading and thinking. I knew I had 2 minutes per question, and after half an hour I was getting a bit concerned about time management. Luckily I got a few shorter questions, and I made up some time. I still only had around 15 minutes left at the end for review. This was a lot less than I would like to have, for a 2:30 exam.

None of the questions seemed to be trying to deliberately catch you out - most of them seemed to be trying to make you think. The number of quick "easy" questions felt pretty low. Maybe that was just me though!

The questions all seemed fair, with few "trivia" questions. One thing that pleased me was that several of the questions were the sort of thing that any experienced network engineer should be able to solve, with a little thought. But if you didn't have any real-world experience, you might come unstuck. That's a good exam to me.

## Exam Quality

* All the diagrams were clear. No problems with fuzzy pictures.
* There was at least one question where I am certain that there was a typo that affected the answer (well...unless I went completely wrong here!)
* Generally the wording was clear, although it could be a lot to take in for some questions.
* Many of the questions had a separate pop-up link to show more detailed output. After two hours, the pop-ups stopped working. Any attempt to open a pop-up gave me a 403 error. My guess is that there was a two-hour timeout on the authentication. Most testing wouldn't pick up the problem, and those candidates who are quicker than I am won't come across the issue. Since I couldn't read all the information, I had to take a guess at what they were trying to ask me, and pick one of the most plausible answers. With multiple choice questions, you can usually discount at least one or two of the answers. I have fed this back to the exam team, so they're aware of the issue. [EDIT: See comments below - this issue has now been resolved.]
* Your blueprint tells you your score for each of the 15 sections, and it also tells you exactly how many questions there were in each of those questions. I quite liked this - I don't think anyone else does it. Other vendors I've tested for (Cisco, Check Point, etc) give you a percentage for each section, but some sections are 3 questions, others are 20 questions.

## Conclusion

It's been well over a decade since I last took an HP exam (HP-UX, no less). It was pleasing to me to see a generally good quality exam, with a somewhat different feel to Cisco exams. I'm undecided if the exam should include simulations - that's not really what they're aiming for with this exam. Possibly they should, to deter the dumpers.

If you have a solid networking background, with experience across HP and Cisco platforms, you should be fine. You can choose if you want to pay for the study guide or not. If you have a lot of experience with configuring mixed networks, you will probably be OK. But make sure you actually understand the underlying protocols.

If your networking background is weaker, you may be better off attending some of the Instructor Led Training run by HP. They run Fast Track courses for those with Cisco experience - these might be more appropriate if you've only ever configured Cisco kit.
