---
author: lindsay
date: 2017-06-04
layout: post
slug: war-stories-always-check-inputs
title: 'War Stories: Always Check Your Inputs'
categories:
- Worklife
series: 'War Stories'
tags:
- war stories
- worklife
---

The extremely irregular War Stories series returns, with an anecdote from 15 years ago, investigating a problem with a web app that only seemed to crash when one particular person used it. Ultimately a simple problem, but it took me a while to track it down. I blame Perl.

## ISPY With my Little Eye

"ispy" was our custom-built system that archived SMS logs from all our SMSCs, aggregating them to one server for analysis. Message contents were kept for a short period, with CDRs stored for longer (i.e. details of sending and receiving numbers, and timestamps, but no content). 

The system had a web interface that support staff could use to investigate customer reports of SMS issues. They could enter source and/or destination MSISDNs, and see when messages were sent, and potentially contents. Access to contents was restricted, and was typically only used for things like abuse investigations. 

This system worked well, usually.

Except when it didn't.

Every few weeks, we'd get reports that L2 support couldn't access the system. We'd login, see that one process was using up 99% CPU, kill it, and it would be OK for a while. Normally the system was I/O bound, so we added alerts to detect high CPU, and restarted it whenever we saw that. It was only every couple of weeks or so, and we had a lot of other things to do, so we put off doing a proper investigation.

## Perl: Write Once, Read Never

The system was entirely custom, and completely undocumented. I had previously moved it from one server to another, but that was mainly a case of copying some content and application folders across. I had never had to dig deeply into it.

There was a bunch of scripts that periodically collected data from the SMSCs, and a Perl CGI app, fronted by Apache. 

Anyone who's ever had to debug someone else's Perl knows that it is a ["Write Once, Read Never"](https://news.ycombinator.com/item?id=2834833) language. Even the person who wrote it has a low chance of being able to figure it out.

I spent days going through the code, starting from the inputs, following through the system, trying to understand how it worked. Along the way I learnt a lot about how it worked, and I cleaned up a lot of redundant code, and various small bugs. The minor improvements were fine, but I still hadn't found a smoking gun.

## A Pattern Emerges: There's Always That One User

The problem persisted, until one day I realized that it was often the same person that told me it wasn't working. There was a team that used the system, so why was it one person in particular? I dug into the logs, and found the culprit:

```
10.64.4.105 - - [10/Apr/2002:08:15:59 +1200] "POST /ispy/sms?src=64220248363 HTTP/1.1" 200 44
10.64.24.33 - - [10/Apr/2002:08:41:59 +1200] "POST /ispy/sms?src=6421606231%20 HTTP/1.1" 200 44
```

The first query was fine. It was the second one that caused problems. At that point the system got stuck in a loop.

Turns out that when this user pasted in a mobile number, for some reason they often ended up pasting in a space at the end.

This completely screwed up the pattern matching code, causing it to get stuck in a loop endlessly cycling through logs. Gah.

## Trust, But Verify

Never trust user input. Never. Always verify that what they've given you matches the format that you expect.

In this case I needed a [`chomp()`](http://perldoc.perl.org/functions/chomp.html) on the input. That stripped off any trailing characters, cleaning up my input. No more crashes, everyone happy. Saved time by not having to restart the app every couple of weeks.

Not that anyone really cared. The app crash/restart cycle had been going on for a couple of years, and the rest of the Ops team just restarted the app without bothering to properly investigate it. Oh well. My fundamental laziness meant more work in the short-term, but less in the long term. I can live with that.

{% include series.html %}

