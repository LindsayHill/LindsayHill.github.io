---
author: lindsay
date: 2013-08-10 06:23:21+00:00
layout: post
slug: black-tuesday-business-as-usua
title: '"Black Tuesday" - Isn''t it Just Business as Usual?'
categories:
- Security
tags:
- microsoft
- patching
- security
---

Microsoft patches are released on a (mostly) monthly cycle, and other vendors have started following suit. When this first happened, people treated it like a major event. But I think that it is now treated as "Business As Usual", and maybe it's time for the headline-writers to realise this.

Ten years ago, patching was a big drama. We didn't have good systems for managing rollouts, a lot of manual work was required, and testing was problematic. As a result, many businesses did very little patching, sometimes with catastrophic results - I have had to deal with major worm outbreaks due to patches not being applied in a timely fashion by $outsourcer, and it's not a lot of fun. The longer you leave patching, the more changes are required, and the higher the risk. So you put it off further...which just makes things worse.

I'm not sure exactly when Microsoft starting bundling their patches into a monthly "[Patch Tuesday](http://en.wikipedia.org/wiki/Patch_Tuesday)", but since at least 2005, most patches from Microsoft have been released on a monthly cycle. This has led to a monthly cycle of news reporting on the patch bundles, often with sometimes hysterical headlines:

> "Nightmare Edition"
> "Microsoft patches IE with record-setting updates"
> "Microsoft Smashes Patch Tuesday Record With Massive Update"
> "Next week's Patch Tuesday could prove to be a real headache for sysadmins"

ISC also publishes a list [of updates released and criticality ratings,](https://isc.sans.edu/forums/diary/Microsoft+July+2013+Black+Tuesday+Overview/16126) with recommendations like "PATCH NOW", "Critical" or "Less Urgent". The idea is that you use this information to decide which updates you should be testing and roll out now, which ones can wait, and which ones you can skip. Headline writers also like to imply that a larger than usual number of patches means sysadmins are extra busy, testing out each individual patch, and every possible combination of patches.

This might have been relevant in the early days, but I just don't see it working that way now. Most organisations I talk to just roll out all the security updates, every month, not long after they get released. They've got good internal tools to manage that patching (WSUS, SCCM, etc), and it's no great drama. Typically there will be a test group of machines that gets all the updates straight away, and the rest get them soon after. Systems are designed with redundancy, and clusters/pairs get split over a couple of patch cycles - e.g. patch one DC tonight, the other DC in a couple of days. There's even a recurring Change Request filed to reboot the servers every month.

It's just not a big deal any more. Microsoft has invested enormous sums in patch testing systems, and by and large, it seems to be working well. We don't see the wide-scale pandemonium people once forecast from applying patches, and the default position for people now is to patch, rather than leave it.


> The monthly patch cycle has become a frequent, repeatable process that everyone's used to, and it's no longer a big drama. Just like all IT changes should be.


Maybe it's time for the journalists to start toning down the language, and recognise this is just business as usual?
