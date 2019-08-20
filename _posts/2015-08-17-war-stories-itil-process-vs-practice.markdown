---
author: lindsay
date: 2015-08-17 06:05:16+00:00
layout: post
slug: war-stories-itil-process-vs-practice
title: 'War Stories: ITIL Process vs Practice'
categories:
- Worklife
series: 'War Stories'
tags:
- ITIL
- war stories
---

{% include series.html %}

Our irregular War Stories returns, with a story about a network I worked on with strict change control, but high technical debt. What should have been a simple fix became far more pain than it should have been. Lesson learned: next time just leave things alone. I'm sure the ITIL true believers loved their process, but did they realise it stopped people fixing problems?

## A classic problem: Duplex mismatch

I spotted a duplex mismatch with one of the services I was responsible for. Throughput was low, and the NIC was showing late collisions. Classic mismatch. Should be an easy enough fix, right? Whoa there son. This is an ITIL shop. No changes without an approved change request!

## Logging Changes: An Exercise in Frustration

Change policy at this company was for a lead time for one week for most systems, or two weeks for some 'important' systems. Changes had to be submitted and approved before the deadline. There was no reason for the delay. Nothing happened during those two weeks, there was no extra review, you just had to wait, because that was the process.

This company had a Change Management system built on top of a main-frame application. Seriously? Yes, seriously. But it was web-enabled! ...sort of. You could go to a web page, and launch a Java applet, that loaded...a green-screen terminal.

The change management application itself was the worst I have ever seen, worse even than the Access Database I used at another site. On login, you were given 5 options. None of those applied. You had to know the secret 4 character sequence, which put you into the change management piece. No clues were given.

The change form included a series of screens, some with free-text fields. When filling in the text fields, it was like using a type-writer. No automatic text wrapping. You had to manually move the cursor to the start of the following line. God help you if you needed to go back & edit text. Dates and special codes were entered in at least 8 different places.

Once you'd finally figured out how to navigate the system, you had to get a colleague to approve it.

Eventually I got there: An approved change.

## The quick part: the actual change

Two weeks later, I go to make the change. Login to the server, find the network adapter configuration, change it to 100/full. Easy, right? Not quite. This ancient Windows version said "Wait right there! I'm going to reboot to apply that change! Bye!"

Crap. I didn't plan on rebooting the server, but it's doing it now. Even Windows 2000 didn't want a reboot. Oh well. It should come back up in a few minutes. Let's set up a continuous ping and wait a while.

Waiting...

Waiting...

## No response...

No response yet. Let's give it a bit longer. You kids won't remember this, but physical servers take a long time to boot.

5 more minutes, and still no response. OK, time to start worrying. I need to look at the server console. What about connecting to the iLO? Nope, don't have one. OK, the DC is just down the hallway. Can I go down & look? Of course not. I have root access to any number of critical systems, but I don't have physical access.

Getting access to the DC needs a special swipe card. How do I get that? Oh, you need a special change request. Shit. My change request doesn't mention physical access.

## Emergency Access time

Crap. Call the boss. He's not far away, and he comes into the office. We start working through what we need to do. Call the NOC, explain that we've had a problem during a change, and need to get physical access. "But you don't have a change logged for tonight. It's tomorrow"

What? I'm sure I've filled in the damned form!

Not quite. The change form had many, many fields, and I'd put the right date in some places, and the wrong date in one of them. No reviewer had noticed that the dates didn't all line up. So now we need to get emergency changes to a change we're not actually supposed to be doing right now.

## Escalate again

In any other organisation, we would have at least just been able to go & look at the server. Not here. We call in the boss' boss. Good thing I've got good relationships with these two. We go through what's required, and somehow Joe figures out the right mechanics to get an emergency change logged, and approved.

Off down to the security guard. Hand in my mobile phone & swipe card. Receive special DC-access swipe card. Back up to the DC.

## At last: Into the DC

OK, finally we're into the DC. Find the rack, flip the KVM over to the right server..."Power supply disconnected: Press F1 to continue." Eh? What's going on?

Get around the back of the machine, trace it out...WTF? There's no cable plugged into the second PSU. Investigate a bit more, realise it's because there's no spare sockets in that cabinet. My guess is that in the past someone needed to plug something else in, so 'borrowed' the second cable from this server.

The boss & I look at each other, wondering what to do. Obviously we can just press F1, the server will boot, and all will be OK. We can figure out the second power cable tomorrow. Except...that would be another change to press F1. We don't have permission to press F1. Our emergency change was just to go and look at the server. We'll need to go and log a change.

Or: "Whoops I think I just leaned my elbow on the keyboard and accidentally pressed F1. Whatever will we tell the Change Prevention team?"

How about: "Yeah, so there's a 30-minute timer on that error message, and if nothing's happened it just continues to boot automatically. The server was working by the time we got into the DC." They brought it, service was restored, and we could go home.

Hours of time spent pissing around, due to one administrative mistake on my part, all because someone didn't bother plugging the server in properly. But hey, I'm sure they filled in the change forms properly, right?

## Process vs Practice

To summarise: The ITIL processes ticked every box. The zealots were happy, and probably thought they were doing a Good Job, making the system better. But had that led to better practice? No, not at all. Look at all the technical failures:

1. No remote access to the server console.
2. Servers running on single power supplies - and no-one detected it.
3. No active monitoring for NIC errors.
4. Outdated server & application software.

The really crushing aspect of the ITIL processes was that they had made it too hard to make changes. No-one was prepared to put the effort into fixing broken things. It was just all too hard. Easier to focus on the new projects, and let the old technical debt keep incurring interest. Only deal with it in an emergency. Such a shame. They'd invested heavily into processes, but hadn't made sure that it matched their practices.

What would be a better use of resources? Tighter change management? Or better technical practices? Which one would lead to better customer outcomes?

Clearly at some point in the past technical management was weak. But if you layer up on process, and make it too hard to fix problems, you won't get quality service.
