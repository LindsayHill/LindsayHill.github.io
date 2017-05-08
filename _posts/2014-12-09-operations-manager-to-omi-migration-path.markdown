---
author: lindsay
date: 2014-12-09 08:19:25+00:00
layout: post
slug: operations-manager-to-omi-migration-path
title: Operations Manager to OMi Migration Path
categories:
- NMS
tags:
- hp
- omw
---

HP has finally announced a migration path for Operations Manager to OMi. It's about time too. This looks like a good path. If you want to stick with HP Software for managing your services, you should be investigating it.

The writing's been on the wall for a while. HP has stopped investment in Operations Manager. I asked last year if HP had [abandoned Operations Manager]({% post_url 2013-07-08-abandoned-operations-manager %}). This year I noted that it was [kicking, but only just]({% post_url 2014-08-15-hp-omw-still-kicking-but-only-just %}). My advice was:


> To customers using HP OM…start planning your migration away from it, if you haven’t already. To customers considering purchasing it: Don’t, unless you’re buying it as part of an overall BSM/OMi implementation, and the salesfolk have guaranteed you can change your licensing over at no cost in future.


Well, HP has finally announced the [OM2OMi Evolution program](http://h30499.www3.hp.com/t5/Business-Service-Management-BAC/The-perfect-pass-The-evolution-of-Operations-Manager-to-OMi/ba-p/6680508). Key points:


  * License entitlement - OM servers can get equivalent licenses for OpsBridge Premium

  * Operations Agent 11 works with both OM and OMi, so you don't have to do the Agent migration at the same time

  * Migration tools to assist with switching over


They do include this quote:


> Well no one at HP is going to try to force you into replacing a product you love. Rest assured Operations Manager will be around for many years to come.


But you're crazy if you believe that. Then again, if you love OM, you might be just a bit crazy. If you're still running HP Operations Manager, you must start evaluating your options. This migration path might just be enough to tip the decision towards going with HP OMi.

**[UPDATE 2015/05/21]** More commentary [here from HP](http://h30499.www3.hp.com/t5/Business-Service-Management-BAC/HPOM-users-here-is-why-and-how-to-move-to-OMi/ba-p/6746769) about migrating from OM to OMi:

> But now with the introduction of OMi 10, HP has released the first version intended to replace HPOM for Windows, HP-UX, Solaris or Linux.
> 
> Although there is no need to immediately move to OMi, it can be considered the successor of HPOM and I think the benefits of its additional features make a compelling case to make the move.
