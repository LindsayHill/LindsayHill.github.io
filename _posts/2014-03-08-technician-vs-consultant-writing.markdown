---
author: lindsay
date: 2014-03-08 02:32:00+00:00
layout: post
slug: technician-vs-consultant-writing
title: Technician vs Consultant Writing
categories:
- Worklife
tags:
- writing
---

Many engineers struggle with business writing. They get easily lost in detail, and produce tortured documents that are technically correct, but of little business value. This is classic "technician" or "engineer" writing. Here's some guidance for producing "Consultant"-style documents will be far more use to the organisations you're working with.

{% include note.html content="Note: This post is primarily about reports and reviews that you are presenting to a customer or management. The rules for internal Wiki-style documentation are a little different. Those should focus on key facts above all else." %}

## Know Your Audience

Know who your audience is, and **know what business problem they're trying to solve**. I can't stress this enough. Everything else should flow from that. The writing will be pitched at the right level, and the customer will be happy because you've clearly helped to solve a problem for them. If you hide your conclusions at the end, or worse yet, don't have any clear conclusion, the customer will walk away confused.

As engineers we love to talk about small technical details, and argue about why Junos is better than NX-OS, or how great it is that the port-to-port latency is only 250ns...but we must be careful to focus on **relevant** details. A 4WD truck may be a better choice for towing a boat than my 1.0L car, but if I don't own a boat, who cares? That database may be limited to 64TB, but if I'm only storing 1GB, growing to maybe 5GB, it's irrelevant. If you're comparing two options, focus on the details that are relevant for this specific situation.

## Layout & Style

Language and layout matter. Be very clear about what you're trying to get across. Guidelines to follow include:

* **Conclusion First: **It's not a murder mystery. I don't want to have wait until the last line to find a conclusion/recommendation. State that up front, with a quick summary of key reasons. That's all most people will read anyway.
* **Bullet Points: **A few bullet points clearly highlighting the key issues are better than long paragraphs where the message gets lost. Short sentences are good.
* **Clear, Strong Statements: **Don't waffle and hide behind language like "I think that consideration should maybe be given to looking at switching from product A to B." Just come out and say it "Product A is the best solution here."
* **Highlight Actions Required: **If you need a specific decision made, or action taken, be very clear about it. Bold, boxed text, highlighted, whatever it takes. Make sure your reader understands what they need to do.
* **No irrelevant detail:** This does not just apply to technical detail. The reader doesn't want a story about what you did while investigating the topic, or hear about how many blogs you read. They want to know the key information, as it relates to the issue at hand.

## Review

Get your document reviewed by someone else. This will improve your writing. There are three different types of feedback:

* **Grammar/Typographical:** "mechanical" errors such as grammar, spelling, formatting. If your document is full of grammatical mistakes, a reasonable proportion of your audience will think less of your technical opinion. Sorry, but that's the way it is. Others may be more forgiving. I am not one of them.
* **Technical:** Review for technical errors - e.g. incorrect description of OSPF LSA flooding. Usually needs someone with strong technical ability in the field.
* **Style and Flow:** Can a reader follow the document, and easily extract the information they need? Do some sections need to be re-ordered/expanded/cut out? This is the sort of thing that a good editor does, and it makes the most difference.

Note that each of these types of feedback are quite different. It's common to use different people for each of them. You might find that sales folks are fantastic at helping get the style and flow right, but they're not detail-oriented, so miss typos.

You will get some reviewers that just say "Yes, it looks fine," without giving any useful feedback. If it's a first pass, and they don't have any feedback...might be worth asking someone else. It's more likely that they didn't review it properly than you got it perfect first time.

## Practice

Keep working at it. These things get easier with time. Pay attention to well-written documents you come across - look at the style, and what you like about it. Read some of the other information out there - e.g. [Greg Ferro](http://etherealmind.com/) has a few good posts about [Design Documentation](http://etherealmind.com/series/design-documentation/). Keep seeking feedback, and keep improving - this will be noticed by your peers, customers, and management.
