---
author: lindsay
date: 2015-05-11 20:00:33+00:00
layout: post
slug: check-point-smartlog-recommended
title: Check Point SmartLog - Recommended
categories:
- Security
tags:
- Check Point
---

_Trigger warning for Check Point haters: I'm about to say nice things about Check Point._

Continuing the recent theme of Check Point-related posts, I'd like to give Check Point credit for once. [SmartLog](https://www.checkpoint.com/products/logging-status-featuring-smartlog/index.html) is what I always wanted from Tracker/Log Viewer, and they're not even charging me extra for it. Shocking, I know.

## Traditional Log Analysis

15-20 years ago, Check Point was well ahead of the competition when it came to viewing firewall logs. "Log Viewer" or "SmartView Tracker,"[^1] let you filter logs by source, destination, service, etc., and quickly see what was happening. The GUI worked well enough, and junior admins could learn it quickly.

Most other firewalls only had syslog. That meant that your analysis tools were limited to grep and awk. Powerful yes, but a bit of a learning curve. There was also the problem of 'saving' a search - you'd end up hunting through your shell history, trying to recreate that 15-stage piped work of art. Splunk wasn't around then.

## Times Change

Tracker has several issues:

* Log files are 'flat' files. It is a proprietary binary format, but it's still flat, with no indexing. The format is very structured, but searches are slow when the files get large.
* Searches are only against the open file. Maximum file size is (still) 2GB. Small shops will do daily log rotation, while large sites will easily have 5 or more 2GB files per day.
* Put those two together, and it's very time-consuming to track down what happened, and when. You need to open the right file(s), and it's hard to see longer-term trends.
* The binary/proprietary nature of the data makes it awkward to integrate with other systems. OPSEC API is still crap, still poorly supported.

In the meantime, we've seen the rise of tools like [Splunk](http://www.splunk.com/), [ELK stack](https://www.elastic.co/products) and [Graylog2](https://www.graylog.com). These have changed the way we view log analysis. We can now easily manage huge volumes of syslog. We can run queries against enormous data sets. We can find & visualise trends in a way that just wasn't practical before. It doesn't matter what tools other firewall vendors offer - we can just chuck the data and Splunk and look at it there. Easy integration with other datasets too.

But Tracker hadn't changed. Still the 2GB file size limit, and still a pain to search across multiple files. Check Point tried to improve it, but it was really showing its age.

## Enter SmartLog

Things have changed. SmartLog was introduced with R75.40. The marketing materials [say it](https://www.checkpoint.com/downloads/product-related/datasheets/smartlog-datasheet.pdf):

> ...enables enterprises to centrally track log records and security activity across all Software Blades with split-second Google-like search results that provides instant visibility over billions of log records.

It is another set of processes that run on your log servers, indexing all incoming logs. Rather than flat files, the data is stored in a way that allows for fast search & retrieval. The original log files are still there - they are unaffected by SmartLog. But now you can run a new client-side application, and quickly search through log data, viewing it in a way that wasn't possible before.

## Enabling SmartLog

You need to be running R75.40 or later on your management server and log server. You should be running R77.20 on those anyway. It's pretty [straightforward to enable:](https://sc1.checkpoint.com/documents/R77/CP_R77_Firewall_WebAdmin/92712.htm#t95391)

* Go to SmartDashboard, and edit the management server object and/or the log server object
* Go to Logs
* Select Enable SmartLog
* Click OK
* Go Policy -> Install Database, and install the DB on the mgmt & log servers

As soon as the database is installed, SmartLog will start indexing logs. It will index all new logs, **and** it will index the current contents of `$FWDIR/log/fw.log`. Depending upon when the log file was last rotated, this could mean the log server CPU spikes for a while. It should settle down later, but you will see higher average CPU usage. It's only on the log server though, so it doesn't matter.

## Using SmartLog

Either cross-launch from another app (Dashboard, Tracker, etc), or directly launch the client-side app SmartLog, and login to your log server or management server.

From there you've got a new UI to work with, but it doesn't take too long to get the hang of it.

[![SmartLog-Screenshot](/assets/2015/05/SmartLog-Screenshot.png)](/assets/2015/05/SmartLog-Screenshot.png)

Enter a search term in the top bar, just the way you think makes sense - e.g `10.1.1.1 drop` to show all drops to/from 10.1.1.1. You can easily build up more complex queries using AND and OR. You can also click on the suggested search filters - time, source, etc.

You can see in that screenshot the automatically identified Top Sources, Destinations, Services, etc. Right-click on any of the items in the main section, and you can choose to add them to the filter - or exclude them. The right-hand side has all the details for each log entry.

The best bit about all this? It's fast. Real fast. Results come back within seconds. It progressively searches further back in time. You'll see current results come back very quickly, and then it takes a few more seconds to go further back, over however far your indexes go. Similar to Splunk, if you've ever used that.

In my tests I was able to search through hundreds of millions of records in around 20s, on average hardware. That is almost impossible with Tracker. Speed fundamentally changes the way you look at your data. You don't just answer your questions more quickly. You ask questions you wouldn't otherwise bother with. Suddenly you find yourself spotting trends, and picking out anomalies in the data.

## Caveats

Original logs are still retained, but SmartLog also indexes the data at `$SMARTLOGDIR/data`. This means that your disk space usage will go up on your log servers. The default policy is to delete index files once there is < 10GB of free space.

Assuming you're using R77.x, you can edit the log server object, and set either a maximum index size - e.g. 100GB, or a time limit - e.g. 14 days. If you change those settings, you'll need to re-install the database. I also found I needed to run "smartlogstop;smartlogstart" for it to apply the new policy.

SmartLog can almost entirely replace Tracker. So I recommend changing your archival/compression policies for traditional logs. Aggressively compress & delete those files, so that you've got more space for SmartLog indexes. They'll be far more use to you.

## Do It

I wouldn't suggest buying Check Point specifically for SmartLog. But if you already own Check Point firewalls, you should enable SmartLog right now. I don't see any downsides, and it will make your life better. You'll also start to get better insight into your traffic patterns, as the speed of use changes the way you look at your logs. Recommended.

---

[^1]: The product name depended upon who had the upper hand at the time - Marketing or Engineering. It seemed to swing back and forth.
