---
author: lindsay
date: 2014-03-03 19:26:04+00:00
layout: post
slug: cciev5-diag-thoughts
title: CCIEv5 DIAG Thoughts
categories:
- CCIE
tags:
- CCIE
---

The CCIE Routing & Switching [v5 blueprint](http://www.cisco.com/web/learning/certifications/expert/ccie_rs/docs/ccieRS_examUpdates4-5.pdf) introduces a new module in the lab exam: DIAG. This section does not have any direct device access, but instead uses a variety of information sources, including some console output.

> …the objective of the new Diagnostic module is to further assess the skills required to properly diagnose network issues. These skills include “analyzing, correlating and discerning multiple sources of documentation” such as email threads, network topology diagrams, console outputs, syslogs and even traffic captures.

My initial thoughts were: “This is the modern version of the [OEQ](http://etherealmind.com/ccie-oeq-waivers/). It’s just another way of stopping the dumpers, by creating a large pool of easily changed questions.” This section is only 30 minutes, with a few questions, but you must pass this section in order to pass the overall exam. Hmmm, OEQ was never very popular, mainly because it seemed to fail a few very good candidates, in addition to the dumpers. One of the problems with OEQ was that it was a free-text field, and you might not correctly interpret the question, or be able to explain yourself succinctly. But look at this:

> Tickets will not require candidates to write anything in order to provide the answer to a ticket. All tickets will be close- ended (as opposed to open-ended) so that the grading will be deterministic, ensuring a fair and consistent scoring.

Aha. That takes away a major source of concern. You can’t get into the same arguments about wording, as you will be choosing answers from a pre-determined list. That makes it similar to the written exam.

Bruno van der Werve gave a presentation at Cisco Live Milan this year about the new blueprint: [BRKCCIE–9162](https://www.ciscolive.com/online/connect/sessionDetail.ww?SESSION_ID=76613&backBtn=true). I strongly recommend that all CCIE candidates watch this session to learn more about the exam. The session spends quite a bit of time going through the DIAG section, including working through some example questions. The video is now available to view for free (registration required).

After watching it, I’ve come to realise that this section is very much “Troubleshooting in the Real World.” Problems are not just based on routers and switches - instead you get a variety of inputs, and you need to sort through them to work out what matters. Look at the possible information inputs:

* Service Desk Tickets
* Email threads
* Console logs showing various command outputs
* Device Configurations
* Diagrams
* Syslogs
* Packet captures (unclear on the delivery method here, but it would VERY cool if they used a [Cloudshark](http://www.cloudshark.org/) appliance

That’s pretty much everything you would have (or get) in the real world, with the exception of Google. It’s up to you to work your way through it, and apply some critical thinking to identify the most likely source of the problem. It looks like you need to recommend the next course of action for some of the questions - obviously you can’t actually test it out, without device access. But this is very much a Real World scenario - frequently we can’t just try something out, as Change ~Prevention~ Management won’t let us.

So how do you study for it? If I was preparing for this lab, I wouldn’t be worried about preparing specifically for this section. I think it needs three things:

1. Foundational Knowledge of Protocols & Cisco Configuration
2. Troubleshooting Experience
3. Real World, practical experience at interpreting problem descriptions, and identifying the best course of action.

The last item will be the most important. All of these skills should be obtained during your study and work experience leading up to the CCIE lab, and should not require any dedicated study.

Hopefully this new section serves its purpose, to correctly identify those who have the knowledge & experience to be called a CCIE, and to weed out those who aren’t there yet.
