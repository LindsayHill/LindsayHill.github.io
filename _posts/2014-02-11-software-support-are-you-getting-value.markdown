---
author: lindsay
date: 2014-02-11 18:00:00+00:00
layout: post
slug: software-support-are-you-getting-value
title: Software Support - Are You Getting Value?
categories:
- Opinion
tags:
- hp
- support
---

Companies pay a lot of money for software support. But do they always get value for it, and do vendors sometimes prolong the "supported" life of a product simply to bank extra revenue? We should hold vendors to account, and demand better.

## What Does Software Support Cover?

Support covers two components:

1. **Break/fix support:** If you strike bugs, the vendor will work with you to track down the issue, and hopefully provide updated code, or a workaround. This support will usually also cover implementation/configuration assistance.
2. **Software updates:** If you have an active support contract, you will be entitled to use all new versions of the product - either minor patches or major releases. Theoretically your support fees partly offset the cost of developing new features and versions. Why I need to pay to fix their bugs is beyond me though.

Support revenues are hugely important to companies - new software sales may vary, but support fees provide a stable stream of income. They are a reliable form of revenue that is highly unlikely to decrease. Companies like Oracle make more in support revenue than new license sales. You make far more from your existing customers than from chasing new ones (another reason software vendors will offer enormous discounts on initial license sales, but don’t discount support fees).

## Software Updates - What Should I Expect?

OK, so we’re helping to fund development of new features and enhancements. What should we expect for that? The release cadence will vary from vendor to vendor, and strategic importance of the item. No-one wants to do a major OS upgrade every year. Point-releases maintaining binary compatibility every 6 months? That’s more acceptable.

But it’s not just about the software itself. Very few products exist in isolation - they integrate with the rest of your applications, and they require Operating Systems to run on. So updates are not just about new features within the software - it also needs to keep up to date with the surrounding environment. It’s no good having a great Windows application that only runs on Windows XP - your vendor needs to take into account changes in that market, and the upcoming EOL date for Windows XP.

Similarly, if you integrate with Microsoft Exchange, or Oracle, you need to keep up to date with major new releases. It’s no good developing a Microsoft Exchange 2000 integration, and leaving it there. I don’t expect new versions exactly in lock-step with all other partners, but surely it is reasonable to see updates within 12 months.

## Storage Essentials - Not Keeping Up

This brings me to [HP Storage Essentials](https://www.hpe.com/us/en/software-solutions/software.html?compURI=1173741&jumpid=reg_r1002_usen_c-001_title_r0001#tab=TAB1). This is:

> …a central console for managing all aspects of storage operations—assets, configuration, topology, capacity optimization, performance management, chargeback, provisioning, compliance and more.

One of the integrations it offers is with Microsoft Exchange. In theory it can map from an individual mailbox right down to the LUN level on the disk array. This should be useful for tracking performance issues with Exchange.

Sounds good, but the problem is that it only supports Exchange 2007 and earlier! Exchange 2010 was released in November 2009, and Exchange 2013 was released in October 2012. It’s now February 2014, and Storage Essentials still doesn’t even support a release that came out over four years ago. I would understand not supporting Exchange 2010 until some time in mid–2010, but to still not support it is unacceptable. Customers are already running Exchange 2010, and planning migrations to Exchange 2013.

What then are we paying software support for, if the product is not being updated to reflect changing environments? Yes, I know it can be tough to update the integrations - but isn’t that what we’re paying for? Is the vendor just taking our money and banking it?

[HP Reporter](https://www.hpe.com/v2/GetDocument.aspx?cc=us&doclang=EN_US&docname=4AA1-6029ENW&doctype=data%20sheet&lc=en&searchquery=&jumpid=reg_r1002_usen_c-001_title_r0001) was in a similar position - until last year it only supported Windows 2003! Looking at the lack of updates for Reporter, and the interface that dates to the mid-90s, we can see HP's clear push to get customers to move towards [Service Health Reporter](https://www.hpe.com/us/en/software-solutions/software.html?compURI=1173047&jumpid=reg_r1002_usen_c-001_title_r0001#.UvMN2kKSym0). Reporter is dead, it just took time before anyone declared it.

But what about Storage Essentials? Is it dead, but no-one’s yet called it? If it’s supposed to be an actively supported product, then it needs to see meaningful updates. As a customer, we have to conclude it is dead, and plan to move away from it. No point paying for updates if they don’t get delivered.

## A Challenge to Vendors and Customers

Vendors, take a look at your product offerings: Still claiming Exchange support, but only up to 2007? That’s not Exchange support. Still claiming Windows support, but only on 32-bit? Time to face facts.

Customers, are you paying software support for products that haven’t been updated in years? What exactly are you paying for? Put pressure on your vendors, and do it in the most effective way possible: Stop paying support if you’re not getting anything for it. Money talks.
