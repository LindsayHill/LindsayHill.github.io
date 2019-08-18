---
author: lindsay
date: 2015-04-08 08:24:52+00:00
layout: post
slug: automate-all-the-things-maybe-not
title: Automate All The Things? Maybe Not
categories:
- Opinion
tags:
- automation
---

I’m fundamentally lazy. That’s why automation appeals: less work for me. Get the machine to do it instead. But automating everything isn’t always the right answer. Sometimes you need to ask yourself: Does this task need to be done at all? Or can I get someone else to do it for me?

Automating tasks carries some overhead. If you’re really unlucky, you’ll end up spending more time on the automation than doing it manually:

![](https://imgs.xkcd.com/comics/automation.png)

So if you can eliminate tasks, you’re in a much better position. Here’s a few contrived examples, based around a fictitious email provider:



## Eliminating Tasks: Maximum Email Size for ‘Special’ Users



15-20 years ago we had limited bandwidth, and limited storage. It seemed reasonable to limit the maximum email message size. Otherwise people would send monstrous 2MB attachments. Of course, there were always ’special’ cases that needed to be able to send enormous 5MB AVI files. So we had special groups of users defined that could send large emails.

Users could put in a request to the Help Desk to get access to send large emails. That would go via some manager, who would of course approve it. Someone would then need to manually update that list of ‘special’ users. Maybe you had some scripts or AD integration to simplify this.

But times have changed. Bandwidth & storage have increased exponentially. Those reasons for limiting email size aren’t really there any more. But no-one’s told the email team to retreat. They’re still implementing the policies that made sense in 1998.

Time to re-evaluate. Time to just have a standard limit for everyone, set to some reasonably high value. Rather than simplify/automate the task, get rid of it altogether. Better for everyone.



## Eliminating more tasks: Profanity Filters



Some businesses still apply profanity filters to email. These block emails that contain words considered “profane.” For some reason HR departments didn’t want people to use any swearwords in email.

But times move on. We’re used to computers now. It’s not a novelty. People use all sorts of language in various other messenger platforms. No-one cares that much anymore. But no-one's brave enough to say "We don't need to apply that filtering anymore." Easy to put in, hard to remove.

You also see problems with people with legitimate names such as “John Cocks.” Daft profanity filters will block all email from that person unless whitelisted.

Again, time to move on. Get rid of the filter altogether, rather than figuring out how to automate tweaking your whitelists.



## Get other people to do the work: Email whitelists



Sometimes you can’t eliminate the task. It still needs to be done. Maybe you need to manually update spam whitelists/blacklists in response to false positives/negatives.

If your current procedure is that end-users have to log a call to get this done, then re-think your procedures. Give users the tools to manage these whitelists/blacklists themselves. Give them an interface they can use. You support the platform, and the users do the work. Perfect.



## Still have to do the work



You can't eliminate all your tasks. You'll still have to figure out how to automate some things. Some stuff you'll still need to do manually. But it's always worth asking the question: Do I need to do this task at all?
