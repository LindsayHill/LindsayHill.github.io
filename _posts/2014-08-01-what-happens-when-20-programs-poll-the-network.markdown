---
author: lindsay
date: 2014-08-01 04:16:02+00:00
layout: post
slug: what-happens-when-20-programs-poll-the-network
title: What Happens When 20 Programs Poll The Network?
categories:
- NMS
tags:
- Python
- sciencelogic
---

Packetpushers [show 198](http://packetpushers.net/show-198-kirk-byers-network-automation-python-ansible/) was a great episode about Network Automation. At one point, Greg asks:


> What happens when you've got 20 apps polling one device?


Well, you might hit the same problem I did:

`SECURITY-SSHD-6-INFO_GENERAL : Incoming SSH session rate limit exceeded`

I have some Python scripts that poll performance and configuration data from a couple of ASR9Ks, and I was getting some gaps in my data. The scripts run on different polling cycles (some hourly, some every 15 minutes, etc). It wasn't consistent, but now and then my script would fail to collect any data.

I dug into it, and found that I was hitting the default SSH rate limit of 60 per minute, calculated as 1 per second. Because I couldn't control the exact scheduling of when my polls ran, I inserted a short random wait timer into some of them. That helped, and I had fewer failures, but it still wasn't quite right.

So I used the [command](http://www.cisco.com/c/en/us/td/docs/routers/crs/software/crs_r4-3/security/command/reference/b_syssec_cr43xcrs/b_syssec_cr43crs_chapter_01001.html#wp1423655881) `ssh server rate-limit 120` to allow 2 SSH connections per second. That has helped, and now I'm not getting any failures.

But it won't be pretty if I do have 20 different apps all trying to poll at once.

(Yes, I know, I should tweak my scripts to better handle the exception, and I should try to figure out why my random wait timers weren't working as expected. But this does what I need for now, and I'm not planning on adding any more polling scripts).
