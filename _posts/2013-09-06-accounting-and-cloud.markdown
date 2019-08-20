---
author: lindsay
date: 2013-09-06 18:00:50+00:00
layout: post
slug: accounting-and-cloud
title: Accounting Models and Cloud Service
categories:
- Opinion
tags:
- cloud
---

Everyone talks about how moving to Cloud-based services can reduce CapEx, and that you only “pay for what you need.” People seem to assume this is a Good Thing, and to my mind, it probably is. But many businesses are obsessed with reducing OpEx, not CapEx. In fact, they will use all manner of tricks to move expenditure to the CapEx bucket. Why is that, and what might it mean for “Pay as you go” models?

{% include warning.html content="Disclaimer: It's been almost 20 years since I studied accounting at high school. Don't take any of this as financial advice." %}

It’s important to understand the difference between Operational Expenditure (OpEx) and Capital Expenditure (CapEx). Both of them are cash going out the door, but accounting practices handle them quite differently. According to Wikipedia, [Operational Expenditure is](http://en.wikipedia.org/wiki/Operating_expense):

> an ongoing cost for running a product, business or system

[Capital Expenditure is](http://en.wikipedia.org/wiki/Capital_expenditure):

> expenditures creating future benefits…incurred when a business spends money either to buy fixed assets or to add to the value of an existing fixed asset with a useful life extending beyond the taxable year

Companies will also typically include the first year's support in the capital cost, and some will even try to write off three years of support as Capital Expenditure. I think that's a scam, but then I'm not an accountant.

The interesting part comes when you have to account for that expenditure. OpEx is immediately classified as an expense. There is no new asset created, and the business capital has been reduced, and our net income is reduced. But if we spent $100k in CapEx, then we’ve still spent the cash, but we now have a $100k asset on our books. Our total capital has not changed, just the nature of the asset mix has changed. But assets have an expected lifetime, and we still want to be able to offset the expenditure against our tax bill. So we can depreciate the cost of that asset over its lifetime. Let’s say it has a three year lifetime, and we expect it to be worth nothing after three years. In that case, we can depreciate it by 33% each year. This depreciation can be claimed against our tax, but it’s being spread out over three years.

OK, but why does this matter? Let’s look at a vastly contrived example: Assume that we have an internal service we deliver, and it costs us $300k to set up, and then $20k/year to operate. A comparable cloud-based service costs $80k/year. Over a three year period, the internal service will cost $360k, while the cloud-based service will cost $240k. Yes, I am ignoring [Cost of Capital](http://en.wikipedia.org/wiki/Cost_of_capital) for now.

So that looks pretty good, right? Overall we spent less, **and** we got to spread out our cash outflow across the three years, rather than having a large upfront payment. But hang on a second. Let’s introduce [EBITDA](http://en.wikipedia.org/wiki/Earnings_before_interest,_taxes,_depreciation_and_amortization) (Earnings Before Interest, Taxes, Depreciation and Amortization). EBITDA is

> ...widely used when assessing the performance of companies.

It's one of the measures people look at it to see what a company might be worth. Note the _D_ there, for Depreciation. So we're excluding Depreciation, and therefore Capital Expenditure from EBITDA. But high OpEx will negatively affect EBIDTA. Another wrinkle: When looking at company performance, people are often more concerned with the trajectory, rather than the absolute numbers - report flat EBITDA, and the share price goes down, even though the cash position might be improving.

Let’s say we went ahead with that move from an internal service to a cloud-hosted one. What happens to our finances? Overall, our cash outlay drops. But what does our EBITDA do? Our costs have moved from CapEx to OpEx. We don’t have the Depreciation any more, and our OpEx has increased, so our EBITDA drops: _Even though we’re actually in a better cash position_. If you’re in management, and EBITDA is one of your key metrics, what happens to you? No bonus for you, I’m afraid. Interesting, isn’t it? What’s the incentive for management to move to cloud-based models? Really, you want to move as much expenditure to Capital Expenditure as possible, so you can improve EBITDA. The very opposite of moving to cloud-based services.

If' we're progressively moving expenditure from CapEx to OpEx, then after a few years things will level out again, and our EBITDA should start increasing again, and all will be well. But we might have flat to negative EBITDA for a few years, and that will look ugly. Just another thing to be aware of when proposing solutions: Organisations have many drivers, and you need to be aware of what those are.

Personally I’m on Warren Buffett’s side, when he [asks](http://www.forbes.com/sites/tedgavin/2011/12/28/top-five-reasons-why-ebitda-is-a-great-big-lie/):

> Does management think the tooth fairy pays for capital expenditures?
