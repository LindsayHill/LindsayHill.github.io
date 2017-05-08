---
author: lindsay
date: 2013-05-18 04:17:17+00:00
layout: post
slug: hp2910al-fails-to-boot-due-to-corrupt-boot-ini
title: HP2910al - fails to boot due to corrupt boot.ini
categories:
- HP IMC
tags:
- hp
- procurve
---

Recently I was upgrading an HP 2910al switch from 14.70 to 15.08.0012. I used IMC to load the new firmware, rebooted the device, waited...and nothing. Device went offline, and never came back. Ruh roh. It was only a lab switch, but still, it meant I needed to go and hook up a console cable, to see what was going on. Luckily the fix wasn't too difficult, and I got things back up and running quickly.

Logging in via the console, I saw this:


```text
=>
```


Hmm. Not good. What happens if I type 'boot' ?


```text
ROM information:
Build directory: /sw/rom/build/samrom(ec_samrom_t4a)
Build date: Jul 15 2011 Build time: 00:07:56
Build version: W.14.06 Build version: W.14.06 
Build number: 47506

Error, /cfa0/boot.ini corrupted; please reboot to console and repair.
Could not open Image file

Bad code in FLASH

Flash memory needs reprogramming or chassis could be faulty. 
Use a PC as the console and perform the update procedure by serial 
Xmodem download of the current Switch Image. If unsuccessful w/ downloading, 
then try replacing chassis.
=>
```


Ugh. That looks really nasty. Luckily this was a [known issue](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?lang=en&cc=us&taskId=110&prodSeriesId=3901671&prodTypeId=12883&objectID=c03394761) , and it wasn't too tricky to get things running again. Using `lshw cfa0` I could see I had a `btm.swi_$TmP$` file. I removed this with `rm cfa0/btm.swi_$TmP$` This gave me more space, but did not resolve the problem with corrupted boot.ini. I removed `/boot.ini` and then the switch was able to boot (using the old software version). Unfortunately this resets your config. There are other ways of solving this, but for me the easiest way to fix it was just to restore a backup from IMC. Once this was done, I tried updating again, and now it's working well, running the new firmware.

{% include note.html content="The script referenced in the HP KB article is supposed to be on an FTP server, but the username/password for that FTP server no longer works. I didn't actually need to use that script though, and I didn't have the Trash-Nebeker directory on my switch anyway." %}

