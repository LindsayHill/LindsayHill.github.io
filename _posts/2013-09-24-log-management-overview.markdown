---
author: lindsay
date: 2013-09-24 19:00:58+00:00
layout: post
slug: log-management-overview
title: Log Management - Overview
categories:
- NMS
tags:
- logs
---

What is Log Management, and why should I care? Can't I just grep through logs when I need to? Isn't syslog on its own enough? This post will give an overview of log management, and in part explain why products like Splunk exist.


## Some Historical Context


Back when @northlandboy was still a...well, boy, we only had a few physical servers. A company with 150 servers was a big network. Applications ran on a few discrete systems. Active/passive was a luxury, and active/active was unheard of. So if I needed to look at the web server logs, there was probably only one server I needed to login to, and one 2MB log file to grep through . Occasionally, I would need to match up logs across a few different systems, and try to correlate them. Hopefully someone remembered to configure NTP.

If I was looking at network device syslogs, we would send them all to one central syslog server, and I could happily use grep/sed/awk to find what I wanted, and massage it into some sort of sensible output. Big syslog servers might have several hundred MB of data. Hopefully I would also have some automated log monitoring in place to raise alerts when Critical conditions were encountered. It was all well under control. But times change.


## Modern Challenges


Fast forward 15 years, and modern networks are very different places. Maybe you've still only got a couple of hundred physical servers...but that's something like 2,000 VMs. Applications are commonly distributed out across many systems. Active/Active across 10 servers is normal. So now when I want to trace a specific transaction, I might have to login to as many as 10 different systems, just to find the right file. Really? And those pesky BAs are getting more annoying, demanding ever more reports, which take me days to gather all the data, run the scripts, and dump it into CSV so the BA can work some Excel magic. Oh and those bloody developers keep asking for production logs, but ITIL processes won't let developers have access to Production systems, so I have to manually copy the logs over for them.

I tried sending all my syslogs to one server, but I just ended up with a 10s of Gigs of logs every day, and trying to grep through that takes forever. I come up with amazing command lines, stringing 15 commands together...and then I forget to save it, and I have to figure out the syntax again next week. There has to be a better way.


## The Rise of Log Management Systems


So we've seen that there was tension for change - the old ways weren't coping any more. A few other factors also came into it - one was the idea of "SIEM" solutions (Security Information and Event Management). People had the idea of pulling ALL of their logs and events into one place, to mine it for security events. The theory was that there was intelligence to be gained from watching patterns of activity across multiple systems. When you think about it, this applies to any form of network or service management - you're looking for anomalies. There's been varying degrees of success with this - many SIEMs only dealt with very structured data, making it difficult to extend them.

Another factor was new technologies such as MapReduce becoming available. Previously we could only use big, slow, expensive traditional SQL databases to store data. The problem is that you had to know the precise forms of data you were storing, and you had to have a good idea about the types of questions you would ask of that data. New technologies made it possible to store large amounts of unstructured data, and ask it all manner of questions, without suffering major performance penalties.

Lastly you also had the growing realisation that there was a lot of hidden value in our data, and that simply watching for alerts wasn't enough - if we could keep all our data, we might later find that there were all kinds of insights to be found, if you could just figure out the right questions to ask.

Pull these factors together, and you get the rise of companies like [Splunk](http://www.splunk.com), who set out to be



> Google for your logs



i.e. fast real-time searching across all your log data.


## State of the Art


So what sort of features can we expect from a modern log management system? Typical features include:


  * Ability to insert wide variety of text-based logs, through many different means - e.g. monitoring logfiles, syslog, TCP, etc.

  * No limits on data structure - unstructured, unknown data is fine

  * Can handle large data sets, with reasonable performance.

  * Distributed workload processing

  * User interfaces that allow quick searching of data, and presenting via a variety of methods

  * Pattern-matching and alerting

Probably the biggest, most well-known player in this market is Splunk, but they're not the only ones. We'll look at them more closely in a future post.

So now all I need to do is to make sure all my logs are going into my log management system, and I can start mining it for intelligence. Let's say I'm dealing with a failed web application transaction. All I have to go on is a customer ID. That customer ID could appear in any number of logs across a range of web and application servers. Now I can go and search for a customer ID in one place, and see all the related logs, across all the related systems. In one place.

Or maybe I can work out the average transaction time, based upon my web logs...and turn it into a graph suitable for NOC display. Or my developers need access to some specific production logs - no problem, set up a limited access account, and give them access to just those logs. Now they can watch them in real-time, and not have to wait for the Ops team to provide the logs.


## The Future


So I've got access to all my logs in a quickly searchable format, and I can perform all kinds of statistical analysis and fancy presentations. Where to from here?

In the short term, I think we'll see the log management systems become more underlying platforms, rather than standalone applications. Their value will be in fast data storage and indexing. They will integrate with other systems, so you'll be using some custom top layer for interaction. That layer will also be pulling data from other areas, such as CRM, NMS, etc. Everyone's got an API these days, maybe we'l actually find ways of pulling it all together?

Talking to customers, the problem has moved beyond "How can I store/process/manage/query this data?" to "OK, I've got something that can answer any question I ask. What question should I ask? How do I gain insight from my data?" I see the next phase as being much more about figuring out how to automatically pick out key trends and anomalies, and gain real insight from operational data. Answer the questions you didn't even know you should be asking. That's a "hard" problem, and it's going to take time to solve. For standard apps, it will be easier - but how can a Splunk developer know what is important about your custom application? That's tricky.


## Do I Need It?


Almost certainly Yes. You just might not realise it yet. There is a learning curve to using these products - not so much in learning the tools, but more in re-learning your own troubleshooting workflows. If you find yourself regularly logging into 20 different systems to look at logs, if you frequently curse Windows for not shipping with grep/sed/awk, if you would just love to give the BA access to the data to run that damn report herself...then you need some form of Log Management. If you've got high-end needs, the prices will get scary - but stick with it, I believe the benefits far outweigh the costs.
