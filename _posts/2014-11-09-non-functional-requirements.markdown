---
author: lindsay
date: 2014-11-09 20:41:37+00:00
layout: post
slug: non-functional-requirements
title: Non-Functional Requirements
categories:
- Opinion
tags:
- architecture
- consulting
- operations
---

I'm currently reading and enjoying "[The Practice of Cloud System Administration](http://the-cloud-book.com/)." It doesn't go into great depth in any one area, but it covers a range of design patterns and implementation considerations for large-scale systems. It works for two audiences: A primer for junior engineers who need a broad overview, or as a reference for more experienced engineers. It doesn't cover all the implementation specifics, nor should it: it would date very quickly if it tried.

I've long disliked the term "[non-functional requirements](http://en.wikipedia.org/wiki/Non-functional_requirement)," so I enjoyed this passage:

> Rather than the term “operational requirements,” some organizations use the term “non-functional requirements.” We consider this term misleading. While these features are not directly responsible for the function of the application or service, the term “non-functional” implies that these features do not have a function. A service cannot exist without the support of these features; they are essential.

It is all the fashion today to separate requirements into 'functional' and 'non-functional,' but the authors are right to point out that this can be misleading. Perhaps it's the old Operations Engineer in me, but if a product doesn't have things like Backup & Restore, or Configuration Management, then it's a non-functioning product to me.

Maybe I should read the term "non-functional requirements" as "The product has to meet these requirements, otherwise it's non-functioning."
