---
author: lindsay
categories:
- Routing &amp; Switching
date: "2024-10-27 20:00 -07:00"
layout: post
slug: juniper-release-process
tags:
- Juniper
title: Juniper Release Process 2024 Redux
---

I've written before about [choosing a Juniper version]({% post_url 2023-04-05-juniper-version-selection %}). Juniper has a new release process. Well, two actually - the new official process, and what they're actually doing...

First the good bits. Juniper started a new [release process in 2023](https://supportportal.juniper.net/s/article/2023-JUNOS-Release-Model?language=en_US). Key points:

* Numbering format remains the same - "\<year\>.\<quarter\>.R\<release number\>-S\<service release>"
* New feature releases are only twice a year, in June & December - "YY.2" and "YY.4". Not quarterly.
* No more "R3" maintenance releases - just the initial R1 release, then a later R2 release.
* Service Releases "-Sx" continue.

I like the new process. It simplifies the versions they have to maintain. We used to say that you should wait for the R3 release, but really there's no difference between R3 and R2-S3. Now Juniper doesn't have to maintain the quarterly releases, and all the maintenance and service releases below them. It avoids the confusion that happened when they kept patching -R2, even after releasing R3.

But here's the thing with a simplified release process: you've got no excuses for not delivering. I have no issue with 6-monthly feature releases. But it feels like we're doing annual releases these days.

Look at the current [download page for the QFX5120-48Y](https://support.juniper.net/support/downloads/?p=qfx5120-48y), a very common ToR switch:

[![24.2R1](/assets/2024/10/qfx5120_242R1.png)](/assets/2024/10/qfx5120_242R1.png)

Hmmm, what's what warning icon telling us? "Use for Lab Qualification only":

[![lab only](/assets/2024/10/lab_only.png)](/assets/2024/10/lab_only.png)

OK, how about 24.2R1-S1, that update should have fixed it? Nope.

[![24.2R1-S1](/assets/2024/10/qfx5120_242R1S1.png)](/assets/2024/10/qfx5120_242R1S1.png)

It is similar for other platforms - the 24.2R1 and 24.2R1-S1 releases are marked for "lab only.". One of the platforms that it is _not_ marked lab only is ACX7100-48L. But in that case I tried using 24.2R1 in production, hit multiple issues, and then JTAC slapped my hand for daring to use that version. That platform does not have any 24.2R1-S1 release published, lab only or not.

The most recent release for QFX5120-48Y is 23.4R2-S2. That's also the ["suggested"](https://supportportal.juniper.net/s/article/Junos-Software-Versions-Suggested-Releases-to-Consider-and-Evaluate?language=en_US#ex_series) release, so it should be good, right?

Well no, not really. Amongst other bone-headed problems, it logs thousands of messages an hour like [this](https://community.juniper.net/discussion/unknown-log-messages) about Unexpected TLVs. All Juniper customers know the standard TAC response "just filter the log messages" - the problem is the rate is so high that you still get a lot of syslog messages like `EVENTD_KERNEL_LOGS_DROPPED [junos@2636.1.1.1.4.82.30 count="2"] Due to excessive logging, (2) kernel events dropped by eventd`

I do not understand how Juniper QA continues to ship code that has huge increases in syslog messages, often at INFO and higher. These are not debug messages. This pattern happens again and again. You'd think "1000x increase in syslog volume" would be cause to investigate, but no.

The most obvious reason for this is the looming HPE acquisition. I know from [firsthand experience]({% post_url 2017-03-19-brocade-update-no-update %}) how tough it is. The official word is always that everything in sunshine and rainbows, but it's never that easy on the ground. The top execs get big payouts to stay, or payouts to leave. But the engineers on the ground have a big fat pile of uncertainty. Many will find a new job, to get greater certainty. Those that stay are distracted, waiting to hear if they still have a job.

I hope it gets better soon.
