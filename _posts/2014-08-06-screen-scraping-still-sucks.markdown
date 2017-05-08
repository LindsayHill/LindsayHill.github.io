---
author: lindsay
date: 2014-08-06 09:18:16+00:00
layout: post
slug: screen-scraping-still-sucks
title: 'Screen Scraping: Still Sucks'
categories:
- Opinion
tags:
- API
- SDN
---

I've written before about "[Why Screen Scraping Sucks]({% post_url 2013-10-10-why-screen-scraping-sucks %})." Well, I can report that nothing has changed. It still sucks. This time I got caught out by the changed behaviour of the "[logging host](http://www.cisco.com/c/en/us/td/docs/ios-xml/ios/esm/command/esm-cr-book/esm-cr-a1.html#wp1503829690)" command.



## Compliance Checks



At a customer site I use [HP IMC](https://www.hpe.com/networking/imc) to perform compliance checks across HP and Cisco networking gear. This has a set of rules that get run against the latest device backups. I have various rules that look for specific patterns - making sure they do, or don't exist, as required.

My systems should all have two log servers defined. The configs should look something like this:


```text
Rack1SW1#sh run | inc ^logg
logging 1.1.1.1
logging 2.2.2.2
```


So I defined an IMC compliance rule that looked for the existence of `logging 1\.1\.1\.1` and `logging 2\.2\.2\.2`. I'm using the Advanced mode, which uses regex matching, so I need to escape the `.`.

This worked well. It alerted on systems that had the incorrect (or no) destinations defined.



## Wait a minute...I thought you said "logging host"?



Turns out that `logging X.X.X.X` was the original form of this command. At 12.3(14)T, Cisco changed the command to `logging host X.X.X.X`. If you read the documentation (and I do), it only refers to the `logging host` form. If you look at the context-sensitive help, it shows two forms:


```text
Rack1SW1(config)#logging ?
  Hostname or A.B.C.D  IP address of the logging host
  buffered             Set buffered logging parameters
...snip...
  history              Configure syslog history table
  host                 Set syslog server IP address and parameters
  message-counter      Configure log message to include certain counter value 
...
```


The interesting thing about IOS is that it often allows multiple command formats, but it will only store one style. Let's look at this for a 15.0 system:


```text
Rack1SW1(config)#logging host 1.1.1.1
Rack1SW1(config)#logging 2.2.2.2
Rack1SW1(config)#do sh run | inc ^logging
logging 1.1.1.1
logging 2.2.2.2
```




## Changes in Default Behaviour



I haven't yet found where this is documented, but somewhere between 15.0 and 15.2, this behaviour changed. Both command formats are still accepted, but now it's stored in the new format:


```text
Router(config)#logging host 1.1.1.1
Router(config)#logging 2.2.2.2
Router(config)#do sh run | inc ^logging
logging host 1.1.1.1
logging host 2.2.2.2
```


Since we were deploying some 15.2 systems, this broke my IMC Compliance checks. It said those routers were not in compliance.

The fix was to edit my compliance checks to look for `logging.* 1\.1\.1\.1"`. By adding the extra `.*` in there, it will match both forms of the command. I could create more specific expressions to match only on `logging X.X.X.X` or `logging host X.X.X.X` but this is good enough here.



## APIs - part of the answer?



It would be much nicer if I could query and API to ask "What log servers do you have set?" and get back a standard format answer.

But of course APIs change too - I just need to make sure I can [handle those changes](http://keepingitclassless.net/2014/08/schema-changes/) well on my side.
