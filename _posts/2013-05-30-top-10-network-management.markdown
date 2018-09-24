---
author: lindsay
date: 2013-05-30 17:00:21+00:00
layout: post
slug: top-10-network-management
title: Top 10 tips for Network Management
categories:
- NMS
tags:
- NMS
---

Many network faults I see are quite preventable, or could have been fixed far sooner, if basic network 'hygiene' had been maintained. Major faults are often the result of multiple small faults. Fix the small problems, and you won't get as many of the big problems. It's not glamorous, and you won't be seen as a hero for solving that production problem, but you will sleep better, and the network will run smoother. Here's my top 10 tips on maintaining good network hygiene:


  1. **Get your naming standards sorted out**. Doesn't matter what standard you use, as long as the name conveys useful information - e.g. what type of device, where it physically is, and perhaps something about where it is logically in your network - DMZ, management network, or internal corporate network.

  2. **Basic monitoring and alerting**: At a minimum, ensure you have basic availability monitoring of all devices in place. Add in CPU and memory monitoring, and throughput monitoring for key interfaces - generally on all switch-switch, switch-server, and WAN links. Yes, we'd all like to be able to run [Statseeker](http://www.statseeker.com) and have reports for every single interface, but it's not practical for most of us. So do what you can. Generally you don't need advanced correlation capabilities either - use your common sense and understanding of the network. Collecting historical data will also help immensely when trying to deal with "the network's running slow" tickets.

  3. **Automate your backups**. Get some tool to do it - doesn't need to be anything fancy, but make sure that you have regular backups of all your configs. This is not just about recovering from device failure - it also lets you see what's changed in your network. If you don't have any money, use [RANCID](http://www.shrubbery.net/rancid/). If you've got a bit of money to spend, use tools like [SolarWinds NCM](http://www.solarwinds.com/network-configuration-manager.aspx) or [HP IMC](http://h17007.www1.hp.com/us/en/networking/products/network-management/IMC_ES_Platform/index.aspx). Bonus marks: Once you do have configurations in one place, you can start analysing them - e.g. check that they have correct SNMP settings. Using grep to search through a directory of text files is a lot easier than logging into 100+ devices.

  4. **NTP**. It costs you nothing, but gives you so much. Enable it, and make sure you set consistent timezones across all devices. Some companies prefer to keep everything in UTC, personally I prefer the local timezone. Doesn't matter, just configure it and be consistent. This will help enormously when trying to track down when events occurred. It's impossible to correlate events when the Cisco syslog says the event happened at 1y4w.

  5. **Logs and traps:** Ensure all devices send syslogs and traps to a central location, and listen to them. They tell you interesting things. Don't ignore them. Consider what it means to receive an STP Topology Change trap: It means you've either got an inter-switch link flapping, or you didn't configure a port properly as an edge port. Either you've got a faulty link, or you're frustrating your users. Fix it. Some of my strategies for log review: Start with a full dump of your logs. Filter out the known, valid entries. You'll find you need to iterate through this several times to reduce the noise. Do this again and again, until you get down to a handful of "unknown" log entries. These need investigating. You should also be looking for the most common entry pattern, and most common source. These either indicate something abnormal (link repeatedly flapping), or something that can be filtered at the source - e.g. do you need to have every switch send a syslog message for every access port? Probably not.

  6. **Duplex mismatches still exist**. Learn the signs to watch for - monitor your interfaces for errors, and act on them. Related to this: Use auto-negotiate. Fixing speed/duplex made sense 10 years ago, but it doesn't any more. Gigabit links must use auto-negotiate, except in very specific circumstances. If you've got a policy of fixing speed/duplex, change it. I don't think I've seen a network yet that is completely free of duplex mismatches. Quietly fix these issues, and you'll get a few less complaints about the network being slow. Doesn't cost you anything.

  7. **Standardise Firmware versions.** All your 3750s should run the same IOS version. All your 2920s should run the same ProVision version. Keep them reasonably up to date too. You don't need to upgrade simply for the sake of it, but you should read the release notes for new versions, and make an informed decision as to whether you need to update or not. The worst mistake I see people do is to take equipment out of the box, put it in the rack, and never even consider what firmware it might be running. The version they installed at the factory is rarely the one you want. Use the same versions everywhere, and all your features and commands will work consistently. Less hassle for you. It's also particularly embarrassing to have a major fault caused by known issues that were fixed years ago.

  8. **Interface Descriptions**. Decidedly unsexy yes, but trust me, putting descriptions on ports will save you in future. Not every single port, but every 'interesting' port - e.g. server-facing switchports, inter-network device links, etc. Some people maintain this information in other locations, but when you're dealing with a fault, the only useful location for that data is on the switchport right in front of you.

  9. **Templates and Fancy Features**. Chances are you're using Cisco kit, and chances are you paid a lot of money for that device - so why use it in the same way you'd use a $100 D-Link device? Modern equipment has many interesting features that can help protect the network and improve service quality. Why not take advantage of them? But don't go overboard - just develop some useful base templates for your configurations that use the key features, and roll out those templates everywhere.

  10. **Be Disciplined. **When you see something that needs tidying up, take it upon yourself to fix it. If you can't fix it immediately, at least make a note of it, so it doesn't get forgotten about. I see far too many engineers who say "Oh yeah, I know about that bit being broken" - but they're too lazy to actually do something about it. No big deal right now, because everything's working. But what if it's a backup link that's misconfigured? Now when you have a fault on your primary link, it goes from being inconvenient, to a major problem.


The ultimate health of your network depends on you. You either put in the steady work, constantly improving things - or you relax for a while, and have it all come crashing down. Do the basics well, and you'll find those big problems stop happening so often.
