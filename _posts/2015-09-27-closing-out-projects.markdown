---
author: lindsay
date: 2015-09-27 03:42:21+00:00
layout: post
slug: closing-out-projects
title: 'War Stories: Closing out Projects'
categories:
- Worklife
series: 'War Stories'
tags:
- war stories
---

{% include series.html %}

We put a lot of energy into new projects. We argue about the design, we plan the cutover, we execute it...and then we move on. But decommissioning the old system is critical part of any project. It's not over until you've switched off the old system.

Years ago I was involved in the buildout of a new network. The new network was a thing of beauty. A clear design, the best equipment, redundant everything. It was replacing a legacy network, one that had grown organically.

The new network was built out. Late one night the key services were cut over, and things were looking good. Everyone was happy, and we had a big party to celebrate. The project group disbanded, and everyone moved on to other things. Since the project was closed out, funding & resources stopped. Success, right?

Except…the old equipment was still running. A handful of applications were left on the old network. Some annoying services used undocumented links between the networks. Even worse, disused WAN links were still in place, and still being billed for.

The problem was that the project was officially ‘over.’ Who’s responsible for finishing off that last bit of cleanup?

I’ve seen similar things in different organisations. The worst case was where they had the 'new' environment, the previous one, and buried in a corner, the first one they ever built. The old ones were still trucking along, with a handful of customers left on each.

This is another form of technical debt. In this case it also has clear financial implications. The original financial model for the project would have included cost savings from reduced support costs, etc. It would definitely have assumed that we wouldn't be running two networks at once. But if we’re still running all the old gear too, will there actually be any cost savings?

Yet this often gets forgotten about. No-one goes back a year later and checks “Did that project actually deliver the savings we thought it would?” The old stuff keeps trucking along, and no-one cleans it up.

In this case I found the time to rip out all the old gear. It was about 12 months after the “official” project close-out when I finally finished. I wanted to send out a “Project is complete!” email, but the boss told me it might not be a good idea politically. Seems that people don’t like to be reminded that they hadn’t actually finished the job.

The project isn’t over when the new system goes live. It’s over when you’ve finally unplugged the old gear.

{% include note.html content="Side note: Don’t forget about following up with the paperwork. If you don’t ask your Service Provider to RQ that unused WAN link, they’ll keep billing for it. Same with hardware support - your vendor’s quite happy to keep charging you support fees for gear you no longer use. Follow up on it." %}

