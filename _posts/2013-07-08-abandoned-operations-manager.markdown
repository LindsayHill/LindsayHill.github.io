---
author: lindsay
date: 2013-07-08 18:00:27+00:00
layout: post
slug: abandoned-operations-manager
title: Has HP Abandoned Operations Manager?
categories:
- NMS
tags:
- hp
- omw
---

HP Operations Manager has been around a long time in the Enterprise server management space. I first started working with it around 2001, and I've always had a soft spot for it, but I'm ready to declare it abandoned. HP has failed to develop the product, and they now seem to be actively working on a viable alternative.

HP Operations Manager (HP OM) is an agent-based server monitoring system. Agents are installed on monitored systems. Monitoring policies are defined at the central server, and pushed to agents. This keeps the bulk of the processing local, as agents only need to raise exceptions. It also makes agents very powerful - they can do anything on a managed node - check processes, monitor logfiles, run actions, etc. HP OM can also act as a Manager-of-Manager, receiving and consolidating events from other systems - e.g. HP SIM, NNMi, Storage Essentials, SiteScope, etc. It has been around in [some form or another since 1993](http://www.softpanorama.org/Admin/HP_operations_manager/index.shtml). It's had a few name changes over the years - OpenView Operations, ITO, VantagePoint Operations, and now just Operations Manager. It has Windows and Unix versions (abbreviated to OMW and OMU/OML respectively). The Windows and Unix variants use the same agents, and can share policies, but the administrative and operator interfaces are completely different.

Development for HP Operations Manager appears to have stalled recently. The last update for OMW (9.0) was in September 2010, and the last update for OML/OMU (9.10) was in November 2010. Since then we have seen further releases for the underlying Operations Agent, and some small patches for Operations Manager, but no real enhancements. HP OM has increasingly ill-suited to modern dynamic environments, and unsurprisingly I hear that sales are well down. Crucially, support renewals are dropping significantly.

HP OM has not adapted well to modern demands. It does not deal well with VMs being deployed at a high rate. It does not offer service monitoring capabilities. It does not offer any way to connect to cloud provider APIs. The agents have continued to be unstable. The administrative interface for OML/OMU looks like something I wrote over a weekend, based on a dodgy PHP shopping cart. It does not look like a piece of software that costs tens of thousands of dollars. Or actually maybe it does - Enterprise software in general tends to be ugly. HP didn't even develop it themselves - they licensed the admin interface from [Blue Elephant Systems](https://blue-elephant-systems.com/). The Java GUI for OML/OMU was a disgrace in 2002 - and it hasn't changed since.

[![](/assets/2013/07/JavaGUI.jpg)](/assets/2013/07/JavaGUI.jpg)

{:.image-caption}
Java GUI circa 2002

[![](/assets/2013/07/JavaGUI1.jpg)](/assets/2013/07/JavaGUI1.jpg)

{:.image-caption}
Java GUI circa 2012


(OK, so I cheated, those images are the same - but that's because it hasn't changed).

I can only assume this lack of development was because they lost out politically, and could not secure the necessary funding and resources. I believe the turning point for HP Operations Manager was the 2006 purchase of Mercury Interactive. This completely reshaped the HP Software division. That portfolio included SiteScope, an agent-less server monitoring system. OM's design and architecture just didn't fit into this model. Efforts to integrate them have been derisory - e.g. the system for using HP OM to manage policies across multiple SiteScope servers is the sort of poor code that I might do as a quick hack. It does not meet the marketing message of "fully integrated."

Also consider this diagram showing how Operations Manager should fit into your overall architecture:

[![Image from www.softpanorama.net](/assets/2013/07/hpom_architecture.jpg)](/assets/2013/07/hpom_architecture.jpg)

{:.image-caption}
Image from www.softpanorama.net

Note how everything feeds into Operations Manager, which then feeds into Operations Manager i (OMi)? To the uninitiated: OMi is a different product, in spite of the near-identical name. When you look at that, you ask yourself - what's the point in HP OM there? Why not just feed directly into OMi?

So what's the future of OMW/OMU? Let's try reading between the lines - look at the recent announcement by HP about [OMi Monitoring Automation](http://h30499.www3.hp.com/t5/Business-Service-Management-BAC/HP-OMi-now-includes-Automation-to-simplify-IT-Monitoring/ba-p/6098861). This separates monitoring policies (configuration, thresholds, etc) from implementation (agent-based, agentless). It bypasses the OM server requirement, with agents directly managed by the OMi server. I haven't seen enough of the implementation details yet to confirm that OM has been completely replaced, but it's clear enough to me what the future direction is. Development has continued for Operations Agent, but clearly Operations Manager is surplus to requirements. Well maybe it all makes sense - why the hell did they ever have two separate products, one named "Operations Manager", and another named "Operations Manager **i"**?

What future for HP OM then? It now only makes sense as a Manager-of-Managers for organisations that are too small to commit to the whole HP BSM suite. Even those organisations need to re-think their use of OM though, as it can't handle a dynamic environment, and stands little chance of being able to integrate proper APM, or cloud service monitoring. There are other products out there that are better suited to modern medium-sized organisations.

To the HP people reading this: Obviously you can't publicly confirm any of this. You've promised ongoing support to those still paying their annual support fees. But if I'm wrong, then show me the code. Deliver some updates to the product, and show us that it is being actively developed. Vague promises of continued commitment mean nothing without shipping code. To customers using HP OM: My advice is to start planning your migration away from it, if you haven't already. To customers considering purchasing it: Don't, unless you're buying it as part of an overall BSM/OMi implementation, and the salesfolk have guaranteed you can change your licensing over at no cost in future.

**[UPDATE 2015/05/21]** If you're still running HP OM, you should actively be migrating to OMi - [HP recommends it](http://h30499.www3.hp.com/t5/Business-Service-Management-BAC/HPOM-users-here-is-why-and-how-to-move-to-OMi/ba-p/6746769), and has plans in place to help:

> But now with the introduction of OMi 10, HP has released the first version intended to replace HPOM for Windows, HP-UX, Solaris or Linux.
> 
> Although there is no need to immediately move to OMi, it can be considered the successor of HPOM and I think the benefits of its additional features make a compelling case to make the move.
