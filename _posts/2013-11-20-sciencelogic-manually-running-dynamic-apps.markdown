---
author: lindsay
date: 2013-11-20 21:58:22+00:00
layout: post
slug: sciencelogic-manually-running-dynamic-apps
title: 'ScienceLogic: Manually Running Dynamic Apps'
categories:
- ScienceLogic
tags:
- dynamic apps
- sciencelogic
---

ScienceLogic Dynamic Applications are policies that describe what data should be collected from managed systems, how it should be collected (SNMP, WMI, API, SQL, etc), how it should be presented, and what alerts should be created. Normally these are scheduled to run periodically, but sometimes you need to run them on an ad-hoc basis, particularly while testing new applications and systems.

This is done by going to the Device Properties page, then the Collections tab, and clicking the lightning bolt next to the dynamic app you want to run:

[![SL Collections](/assets/2013/11/SL-Collections.png)](/assets/2013/11/SL-Collections.png) [![SL Dynamic App Output](/assets/2013/11/SL-Dynamic-App-Output.png)](/assets/2013/11/SL-Dynamic-App-Output.png)

When you do this, it instructs the appropriate collector to go and run that Dynamic Application, and report back the results. This might take a minute or two to run, as the database needs to connect to the relevant collector, tell it to execute the application, and wait for the results. If you have Collector Groups, the collection will be run by whichever collector is currently assigned to that device.

This is normally OK for simple tests, but if you need to do it a few times, or for a few different dynamic applications, it can be a little slow and annoying. Wouldn’t be faster if we could just manually run the collection directly from the collector?

Turns out we can, by SSHing to the collector, and running:

```sh
/usr/local/silo/proc/dynamic_single.py did aid
```

This will immediately run and report the results. You need to know what the device ID is, and the ID of the specific dynamic application. These are circled in the earlier diagram.

Here’s some example output from one of my systems:

```sh
[root@collector ~]# /usr/local/silo/proc/dynamic_single.py 368 250
Collecting for App_id: 250 from did: 368
Using credential: 50
Created an app object: AID: 250 [SNMP CONFIG] / DID: 368
Created a dev object: 368
Will use 10.1.1.1 as IP for device
AID: 250 [SNMP CONFIG] / DID: 368 has 1 objects ready to collect
Precollect for app returned True
Oids to collect:
GROUP: 1 obj_id: 2913 OID: .1.3.6.1.2.1.25.3.3.1.1 NAME: hrProcessorFrwID
Starting collect
collecting on object 2913.  len bad_oids = 0
SNMP WALK on .1.3.6.1.2.1.25.3.3.1.1
result of walk before oid indexing: [('.1.3.6.1.2.1.25.3.3.1.1.4', '.0.0'), ('.1.3.6.1.2.1.25.3.3.1.1.5', '.0.0')]
oid_result = .1.3.6.1.2.1.25.3.3.1.1.4, oid index = .4
oid_result = .1.3.6.1.2.1.25.3.3.1.1.5, oid index = .5
result of walk after oid indexing: [('.4', '.0.0'), ('.5', '.0.0')]
result for object 2913: [('.4', '.0.0'), ('.5', '.0.0')]
Collect took: 0.0307250022888 seconds
AID: 250 [SNMP CONFIG] / DID: 368 thinks it was collected properly
AID: 250 [SNMP CONFIG] / DID: 368 had 0 bad oids (out of 1): []
Original Collected index of .4 was mapped to EM7 internal instance of .4
SNMP: Original Collected index of .4 was mapped to EM7 internal instance of .4
Original Collected index of .5 was mapped to EM7 internal instance of .5
SNMP: Original Collected index of .5 was mapped to EM7 internal instance of .5
BAD OIDS: 0
ERROR MESSAGE:
COLLECTED: True
FOUND OBJECTS: set([2913])

Group  Obj_ID Index     Data                          Changed? Collection Time
________________________________________________________________________________
1      2913   .4        .0.0                          0        3.437ms
1      2913   .5        .0.0                          0            --

processing app alerts
Loaded 0 active alert policies for this app
loaded app default thresholds: {}
loaded device specific thresholds: {}
Final alert thresholds for this collection: {}
Loaded currently active alerts: {}
No internal alerts (app errors).
Done!
[root@collector ~]#
```

I haven’t seen any public documentation for this command, so I don’t know if there are any other options you can select when running it. Note that you have to run it from the collector that is currently assigned for that device. If you have multiple Data Collectors in a Collector Group, and try to run it from a different collector to that currently assigned, it will report back that:

```sh
Device <X> is not aligned with dynamic app <y>
```
