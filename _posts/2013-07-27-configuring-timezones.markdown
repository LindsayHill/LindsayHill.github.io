---
author: lindsay
date: 2013-07-27 01:43:24+00:00
layout: post
slug: configuring-timezones
title: Configuring Timezones on IOS, ProVision and Comware
categories:
- NMS
tags:
- Comware
- IOS
- logs
- NTP
- procurve
---

Timezones can be complicated things - every country has its own set of rules around how to decide what time it currently is - and those rules change, frequently. Modern Operating Systems have systems in place for handling those changes. Give it a location, it will deal with the rest. Unfortunately, network devices don't have those systems, and we need to explicitly tell them how to work out what the current local time is, and when to change it to Daylight Savings Time. Here's how to do that for Cisco IOS, HP ProVision, and HP Comware systems.

For our examples below, we're going to use New Zealand, for no better reason than that's where I live. In New Zealand, our regular timezone is NZST, or "New Zealand Standard Time". This is UTC+12:00. During summer, we switch to NZDT, or "New Zealand Daylight Time". This is UTC+13:00. We change from NZST to NZDT on the last Sunday in September at 2:00, and we change back on the first Sunday in April at 3:00.

{% include warning.html content="Note: I'm in the Southern Hemisphere, so my start and end months for DST are flipped, as compared to most readers, who are in the Northern Hemisphere. Pay close attention when you're configuring this for your own timezone" %}


A quick background on timezones: computers store their time in seconds since the epoch, or seconds since January 1, 1970. This is the number that is used when synchronising systems with NTP. They then use a set of rules to work out what that time converts to in your local time. Based upon the date, they can also work out if you're currently in normal time, or daylight savings time. On multi-user systems, you can even define individual timezones for users - it doesn't matter, since the time that users see is simply an interpretation of the underlying system time. As long as you're using the right rules for that user, it will display a human-readable time, in the timezone they expect. Changing the timezone rules just changes what's displayed - it doesn't really change the underlying time.



## IOS Timezone Configuration



IOS will default to UTC time. We need two commands - the first will set the name our of normal timezone, and our offset from UTC. Note you don't need to use "+" if it's a positive offset. The second command defines the name of our summer timezone, and provides rules on when this timezone applies. Here's how it looks on my system:


```text
Cisco#show clock
00:25:31.275 UTC Sat Jul 27 2013
Cisco#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Cisco(config)#clock timezone ?
  WORD  name of time zone

Cisco(config)#clock timezone NZST ?
    Hours offset from UTC

Cisco(config)#clock timezone NZST 12 ?
    Minutes offset from UTC

Cisco(config)#clock timezone NZST 12 
Cisco(config)#clock summer-time NZDT ?
  date       Configure absolute summer time
  recurring  Configure recurring summer time

Cisco(config)#clock timezone NZDT recurring last Sun Sep 2:00 first Sun Apr 3:00
Cisco(config)#^Z
Cisco#show clock
12:25:51.341 NZST Sat Jul 27 2013
```


A few points to note:


  * The actual timezone name is a free-form text field. You can call it whatever you like. Some people use lower case here. I like consistency and accuracy, so I will always use the official timezone names.

  * You can specify partial-hour offsets, but you don't have to enter anything if it's a whole hour offset.

  * Summer Time rules can be set as a once-off (using "date"), or they can be recurring. You can't mix these though! Some countries have had once-off changes - e.g. NSW, Australia during the 2000 Summer Olympics. You can specify once-off rules, but IOS will remove the "recurring" line. OSes like Windows and Linux allow much more complex rules, so you can have different rules in each year. With IOS, you'll just need to push out new rules using your config management tools. No big deal, right?

  * If you're really keen, your summer time can be an offset of any number of minutes between 1 and 1440. The default is 60. I don't know any timezones that actually implement rules where they change by more than one hour.


All up, it's pretty simple to configure - you just need to figure out what rules apply to your area. The ability to specify rules like "first Sunday" make it straightforward, as that is usually how it will be written in local legislation.


## Procurve Configuration


HP Procurve systems have a slightly different configuration method. You can specify your location, if you're in the US. The other 6.5 Billion of us need to specify our offset in minutes from UTC. You don't specify the timezone name either, just the offset in minutes. The summer time rules are a little different too - it tries to give you options for various locations, but you can't really trust these. Better to define the rules manually:


```text
Procurve(config)# clock
 set                   Set current time and/or date.
 summer-time           Enable/disable daylight-saving time changes.
 timezone              Set the number of hours your location is to the West(-)
                       or East(+) of GMT.

Procurve(config)# clock timezone
 gmt                   Number of hours your timezone is to the West(-) or
                       East(+) of GMT.
 us                    Timezone for US locations.
(config)# clock timezone gmt +12:00
Procurve(config)# show run | inc timezone
time timezone 720
Procurve(config)# time daylight-time-rule
 none
 alaska
 continental-us-and-canada
 middle-europe-and-portugal
 southern-hemisphere
 western-europe
 user-defined
Procurve(config)# time daylight-time-rule user-defined
 begin-date            The begin date of daylight savings time
 MM/DD[/[YY]YY]        New date
 end-date              The end date of daylight savings time
 HH:MM[:SS]            New time
 timezone              The number of minutes your location is West(-) or
                       East(+) of GMT

sw01pi(config)# time daylight-time-rule user-defined begin-date 09/24 end-date 04/01
```


It's a little annoying that "show time" on Procurve doesn't display the local timezone. Note that I could define the local timezone and the DST rules in one line, but I've split it in two for readability. It will split in two for the displayed configuration too. You can also see in the above output that there are few different ways of defining the local time - either using `time timezone` or with the `clock timezone` command.

The really frustrating part of Procurve timezone configuration is its very limited capabilities for defining summer time. You can't specify a rule like "last Sunday" - only an exact date. What the switch then does is look for the "first Sunday on or after that date" - so to be the first Sunday of the month, you can specify 04/01, as I have. To be the last Sunday of the month, you need to specify the date that is "number of days in the month - 6". You can't specify a time of day either - it will always change at 2:00, which is not always correct. Very poor. Note that if you use the predefined rules for the Southern Hemisphere, it will use the last Sunday in October to start, and the first Sunday in March to end. Not much use in New Zealand.


## Comware Timezone Configuration


Comware configuration looks more like Cisco configuration. Surprise, surprise. Here's a typical setup:


```text
dis clock
13:23:49 NZDT Sat 07/27/2013
Time Zone : NZDT add 12:00:00
Summer-Time : NZDT repeating 02:00:00 2012 September last Sunday 03:00:00 April first Sunday 01:00:00
dis cur | inc clock
 clock timezone NZDT add 12:00:00
 clock summer-time NZDT repeating 02:00:00 2012 September last Sunday 03:00:00 April first Sunday 01:00:00
```

The rules are pretty similar to IOS - you have the same level of flexibility. You can specify one-off rules if required. Note you need to specify a year for the repeating rules, but after that it will apply for every year.

So there we have it: How to configure the right timezone settings across IOS, Comware and Procurve. Obviously you'll need to figure out the exact settings for your local timezone, but now there should be no excuse for displaying the wrong timezone. Coming up: Using NTP to make sure you've got the right underlying time, and making your logs actually show the timestamp, not "1y4w".
