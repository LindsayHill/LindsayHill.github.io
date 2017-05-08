---
author: lindsay
date: 2014-10-18 03:31:28+00:00
layout: post
slug: disappointed-with-check-point
title: Disappointed With Check Point
categories:
- Security
tags:
- Check Point
---

I have recently started working with Check Point products again, after a 5-year break. This has given me a different perspective on how they are progressing. It has been disappointing to see that they're still suffering from some of the same old bugs. Some of the core functionality is now showing its age, and is no longer appropriate for modern networks.

When you’re using a product or technology on a regular basis, it can be hard to accurately gauge progress. Maybe it feels like there are only incremental changes, with nothing major happening. But then you come across a 5-year old system, and you realise just how far we’ve come. If you don’t think iOS is changing much, find some videos of the first iPhones.

The opposite is when it feels like there are many regular enhancements…but when you step back you see that core product issues are not dealt with. It can be hard to see this when you’re working at the coal-face. You need to step away, work with other products and systems, then return.

That’s what I’ve done with Check Point recently. Through much of the 2000s, I did a huge amount of work with Check Point firewalls. But for the last 4–5 years, I didn’t log into SmartDashboard once. I changed jobs recently, and now I’m up to my elbows in firewall policy changes again.

Stepping away from Check Point for a few years has given me a different perspective, and I’m disappointed by what I’ve seen. I was expecting to see more progress, but it doesn’t seem to have happened.



## Functionality Challenges



The biggest issue I have right now is that the management interfaces seem to actively resist automation. There are some methods for scripting bulk changes, but it is still too limited, and unsuitable for use by regular admins. Too many change requests are still of the form “Go to about rule 500. Add a new rule with this source and destination. Install policy.”

This is the way it always was, and I thought that was OK in 2002. But in the years since I’ve learnt the value of automation, and repeatability.

The default SmartView Tracker (aka Log Viewer) is far better than the options available with most other firewalls, but it really struggles under load. Files still roll over at 2GB. That seemed like a large file in 2001, but we moved on. [SmartLog](http://www.checkpoint.com/products/logging-status-featuring-smartlog/index.html) should be the default for all systems now - I don’t know why Check Point still has the traditional logging systems.

OPSEC is still poorly documented, and still a major pain to try to integrate with other non-Check Point products.



## Same Old Bugs



There are still too many core product bugs that haven’t been properly resolved. Yes, there are many extra features that have been added, but I expect that core functionality should be nailed by now. Problems I’ve seen include:




    
  * Management clients crash once or twice per week. Still.

    
  * ClusterXL is still flaky, with clusters going into split brain. This is not a new feature - it has been around for well over 10 years.

    
  * Firewalls stop logging remotely, and need a cprestart to fix. I have logged several cases about this over the years, but Check Point seems unable/unwilling to fix it.

    
  * CPD & FWD memory leaks. Still. It seems that every single patch includes fixes for memory leaks in CPD. But it still leaks.

    
  * File corruption on gateways. How does that happen?

    
  * NIC drivers - still far too many driver issues causing poor throughput/high CPU.

    
  * IPS - this is still untrustworthy. Updating signatures is still very scary, with good reason - there’s still a high risk of blocking legitimate traffic.



All these are core features, and should be well under control. It is disappointing to see the same bugs you first saw more than 10 years ago.



## It’s Not All Bad



Some things have improved - e.g. Check Point manuals are now available in HTML as well as PDF format for new releases. KB articles are restricted, but they always were.

Gaia is showing promise too. In the early days it seemed like a good idea being able to choose your underlying OS to run Check Point on. But now we want to remove variability, so consolidating on Gaia is a good thing.

Identity Awareness (allowing rules to be defined using Users, not IP Addresses) seems to work pretty well. This gives Check Point a chance of competing against Palo Alto.

I hear that R80 (the next major release) will include major overhauls of the underlying management systems. This should help with automation and stability. But the first few releases will be full of pain.



## Will it get better?



Yes, Check Point will get better in future. But will it develop at the pace I want to see, and will they resolve core issues? Or will they just add more pointless “blades” to try to extract more licensing revenue? I’m not confident.
