---
author: lindsay
date: 2014-08-15 18:30:01+00:00
layout: post
slug: hp-omw-still-kicking-but-only-just
title: 'HP OMW: Still Kicking, But Only Just'
categories:
- NMS
tags:
- hp
- omw
---

A year ago I asked "[Has HP Abandoned Operations Manager?]({% post_url 2013-07-08-abandoned-operations-manager %})" There had been no significant development for a long time, and the signs were that HP was moving away from OM to OMi.

Last week HP made a move that confirms my original thinking: It's dead (it just doesn't know it yet). HP released a [Customer Letter](http://support.openview.hp.com/pdf/omw-9_0x-omw-basic-suite-9_1x-ext-customer_letter.pdf) announcing an extension to the "End of Committed Support" date, from December 31, 2016 to June 30, 2018:

> HP is committed to providing the highest level of customer care to you **while you determine your future strategy** for your HP Operations Manager for Windows 9.0x & HP Operations Manager for Windows Basic Suite 9.1x products.

_(**emphasis mine**)_

That's right, no new version announcement, just extending support for the current version. Implication: no new versions coming any time soon.

## Applying a few volts to OMW 9.0

HP has released patches [OMW_00185](http://support.openview.hp.com/selfsolve/document/FID/DOCUMENTUM_OMW_00185) and [OMW_00187](http://support.openview.hp.com/selfsolve/document/FID/DOCUMENTUM_OMW_00187) for OMW 9.0. These include the usual bugfixes, and these enhancements:

* Web console enhancements resulting in feature parity with the MMC console while offering significant performance advantages
* Management Server platform support extension to Windows Server 2012 and Windows Server 2012 R2
* MMC Console support extension to Windows Server 2012, Windows Server 2012 R2, Windows 8.0 and Windows 8.1
* Browser support extension to Internet Explorer 10, Internet Explorer 11, and Firefox 24 ESR

So they're doing just enough to keep the platform going, but not really making any great changes.

{% include note.html content="Side note: Why bother making the MMC compatible with Windows 8/2012 if the web console now has feature parity?" %}

## The Future is OMi

The support alert I received via email included this quote:

> To bring your monitoring solution to the next level, we also encourage you to take a closer look at our next generation operations management solution, HP Operations Manager i ([www.hp.com/go/omi](http://www.hp.com/go/omi)), with its differentiating capabilities in automating monitoring, determination of root cause and getting immediate insight into the business service impact. HP Operations Manager i can be deployed as manager of manager on top of your monitoring solution or as the evolution path of your today's HP Operations Manager implementation, ensuring that your investment is protected.

So like I said last year, if you're using HP OM and want to stick with HP, go to OMi. My advice remains:

> To customers using HP OM...start planning your migration away from it, if you haven’t already. To customers considering purchasing it: Don’t, unless you’re buying it as part of an overall BSM/OMi implementation, and the salesfolk have guaranteed you can change your licensing over at no cost in future.
