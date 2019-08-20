---
author: lindsay
date: 2015-06-07 05:10:53+00:00
layout: post
slug: brocade-certified-vrouter-engineer
title: Brocade Certified vRouter Engineer
categories:
- Routing &amp; Switching
tags:
- Brocade
- NFV
- SDN
---

If you've visited the [Brocade](http://www.brocade.com/) website recently, you've probably seen the "[Free NFV Certification](http://www.brocade.com/forms/jsp/nfv-certification/index.jsp?src=WS&lsd=BRCD&lst=Banner&cn=SDN-GDG-14Q3-CERT-NFV%20Cert)" banner. I signed up for this several months ago, but had put off completing the course. I had a little downtime recently prior to starting [work at Brocade]({% post_url 2015-05-21-the-next-step-brocade %}), so I completed this course & exam. Here's my impressions.

{% include note.html content="Disclaimer: I now work for Brocade. Assume what you will about my biases. These are my opinions, not my employer's." %}

## What's the Course/Exam About?

From the [official documentation:](http://www.brocade.com/education/certification-accreditation/certified-vrouter-engineer/index.page)

> As a Brocade Certified vRouter Engineer, you must be able to demonstrate the ability to install, configure and troubleshoot features of Brocade Vyatta Network OS.

i.e. it's primarily about the basics of Vyatta.

## What's Included?

Here's what you get when you sign up:

1. A download link to the Brocade Vyatta 5400 vRouter image.
2. Access to the "Brocade Certified vRouter Engineer Course."
3. Links to certification materials and communities.
4. Promo code for the BCVRE exam.

Note that you can run the Vyatta image on a local hypervisor, or if you prefer you can select it from the [AWS marketplace](https://aws.amazon.com/marketplace/pp/B009I5TLOE/ref=srh_res_product_title?ie=UTF8&sr=0-2&qid=1432685267612). Personally I ran it on VMware Fusion on my laptop. It's light on resource, so you can easily spin up several vRouters.

## The Course

The course is delivered as a series of 10 approximately half-hour modules. These are Flash-based, with slides, audio, and sometimes PDF attachments. It's delivered through the [Saba](http://www.saba.com/us/) platform. You may have come across it elsewhere.

The modules cover installation, basic system configuration, Ethernet + IP setup, DNS, DHCP, simple OSPF, static routing, firewall configuration, NAT, logging, etc. They're all pretty straightforward, and the time chunks are good - you can easily knock off a module when you have a little spare time.

I recommend running Vyatta at the same time as you watch the modules, so you can configure the features yourself. Better yet, try to break those features. I also frequently paused the videos to make a few notes in flashcards.

[Brocade's website](http://www.brocade.com/education/certification-accreditation/certified-vrouter-engineer/curriculum.page) has a full list of the modules, with more detailed descriptions.

## The Exam Itself

The exam (170-010) is proctored through [Pearson Vue](http://home.pearsonvue.com/Home.aspx), at the usual test centres. The exam would normally cost $150USD. Use the promo code Brocade provides, and it magically becomes free.

The exam is a mix of multi-choice (single or multiple answers) and drag & drop questions. No simulation questions. I had 51 questions in 75 minutes. This was plenty of time. You'll be asked to interpret diagrams and/or the output of various Vyatta commands. The questions are straightforward - I didn't feel like there was any 'trick' language. None of the triple-negative questions that some vendors have used in the past. Note you can go back and review questions (unlike Cisco exams).

The questions followed the [published exam objectives](http://community.brocade.com/t5/Certification/Brocade-Certified-vRouter-Engineer-Exam-Information/ta-p/3899). Read through those, and make sure you understand them. Note that several of the areas are generic networking questions, related to Ethernet & TCP/IP. Nothing over the top - you'll be fine with those if you've been working in networking for any length of time.

The course materials cover most of what you need, but you will want to spend a bit of hands-on time with Vyatta to help the concepts sink in. You don't need to spend weeks preparing for this exam though. If you have good networking experience, you'll only need to spend a few days on it. It does give you a good excuse/reason to try out Vyatta though!
