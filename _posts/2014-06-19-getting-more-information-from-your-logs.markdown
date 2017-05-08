---
author: lindsay
date: 2014-06-19 09:13:22+00:00
layout: post
slug: getting-more-information-from-your-logs
title: Getting More Information From Your Logs
categories:
- NMS
tags:
- logs
---

[Packet Pushers](http://packetpushers.net/) normally focuses on networking, but episode 192 covered "[Logging Design and Best Practices.](http://packetpushers.net/show-192-logging-design-best-practices/)"  I often think about logging in the context of network management, so it was good to hear an episode devoted to it. There's a lot of valuable information in logs, and it's an important part of managing your network.

There's a lot to getting valuable information from your logs, but just I wanted to pick up on two key points/techniques mentioned on the show:



  1. Exclude what you know

  2. Statistical analysis


People focus too much on looking for specific patterns in the logs, which was why I was pleased to see these techniques mentioned.



## Exclude What You Know



It's easy to look for specific patterns - e.g. **LINK_DOWN**, **LINK_UP**, **FAN-FAIL**. But we need to know what these patterns are in advance. What about what we don't know? That's where things slip through. Sometimes we need to go through our logs, and strip out everything that is just "business as usual," or regular "noise."

Start by looking at the logfiles, and look for common patterns. Remove those, look for the next most common pattern. Rinse and repeat until you're down to a few entries that don't quite belong. Do this regularly, and you'll find a few new things that would have slipped through the cracks otherwise.

In the early days, I would do this by starting with the raw syslog file, and using `grep -v  | grep -v  | ...`. Tedious stuff.

Times have moved on. Use a tool like Splunk/Graylog2 to set up a saved search that has all the patterns to exclude. Add that to a dashboard, and you can quickly review it every few days. You'll need to keep tweaking the search to improve it. Do this every time you get a few new entries appear.



## Statistical Analysis



You don't even need to look at the log contents to learn things. You can infer things just by looking at the rate at which you receive logs from devices. Here's some ideas on things to look for:


  1. Highest logging rate (all patterns)

  2. Most common pattern (across all sources)

  3. Lowest logging rate (e.g. sources no longer sending logs due to fault/misconfiguration)

  4. Overall logging rate - higher or lower than normal.

  5. Abnormal rate for a specific source (higher or lower than normal)

  6. Abnormal rate for a specific pattern.


The first four are easy to do with any log management tool. The last two can be a bit trickier. It needs statistical analysis, but tools that promise to do it automatically can be pretty noisy. Sometimes you just end up looking at a time-series graph, and eye-balling it.

There's a lot of data in the logs, you just need to know how to look for it. Go and listen to the episode now - you've already subscribed, right?
