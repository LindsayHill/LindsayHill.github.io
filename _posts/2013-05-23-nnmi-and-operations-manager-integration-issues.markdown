---
author: lindsay
date: 2013-05-23 18:00:47+00:00
layout: post
slug: nnmi-and-operations-manager-integration-issues
title: NNMi and Operations Manager integration issues
categories:
- HP NNMi
tags:
- hp
- nnmi
---

Watch out if you have HP NNMi integrated with HP Operations Manager, and you're upgrading to the latest Operations Agent. I have a customer using HP Network Node Manager 9.22, with HP Operations Manager for Windows 9.0. Both of those systems are running the latest publicly released patches. They are integrated using the Operations Agent method, where NNMi sends SNMP traps to the local Operations Agent on port 5162, which then forwards NNMi incidents as Operations Manager messages.

Previously we were running Operations Agent 11.03. I wanted to upgrade to the current Operations Agent version, 11.11. Should be all good, right? Because surely if I'm using the latest versions of everything, and using the recommended integration methods, it will all work, right? Sadly no.

The agent install went fine, but shortly after that we started getting errors about the `opctrapi` process restarting. After a certain number of automated restarts, Operations Manager refuses to restart it. This process is the one that listens for SNMP traps. On our system, it listens for SNMP traps on port 5162. NNMi forwards traps to this port on the NNMi server.

Looking in HP's knowledge base ([support.openview.hp.com](http://support.openview.hp.com)), I found several references to `opctrapi` instability with the latest agent. Talking to support, there are hotfixes available. Of course, you can't just have one hotfix - no, once you request that, you find that it's dependent upon yet another hotfix. More hassle than I care to deal with. Managing multiple hotfixes becomes a configuration nightmare, and is just one of the reasons I'm 'over' agent-based monitoring. That's a subject for another time anyway.

Digging further, I found that this system was set to use `SNMP_SESSION_MODE NNM_LIBS` It turns out that the Operations Agent can use several different methods to listen for SNMP traps. It can use NNM libraries, it can use Net-SNMP libraries, or it can subscribe to the Windows SNMP trap service. Why on earth they have so many different methods is beyond me. You can find out what you're using with `ovconfget eaagt`

Turns out this system was using `NNM_LIBS` - this tells the agent to try and subscribe to the NNM 7.5 PMD daemon. This server has never run the 7.5 version of NNM, so I'm a little surprised this even worked in the first place. I didn't set this one up myself, so I don't know the history. But it did try and use an old method, and it doesn't work with the newest agents. I changed it to use NETSNMP, with `ovconfchg -ns eaagt set SNMP_SESSION_MODE NETSNMP` I then restarted the trap interceptor with `ovc -start opctrapi`

So far, the processes have been stable, and I can avoid having to apply hotfixes. This makes my life easier, but it's one of those things that makes you wonder about our whole monitoring approach. HP sells these products as "fully integrated", but when you actually dig into it, there's a bunch of manual steps you have to carry out, and it's just SNMP traps underneath it all. The same thing we were using 20 years ago, hidden a bit, and dressed up, but still just plain old SNMP traps. Customers shouldn't have to configure any of this - they should just tick a box, and say "yes, I have Operations Manager at this address." Or even better, just have one system to monitor network and servers, not separate tools for everything. How can you get true event correlation, when you have fundamentally different tools monitoring storage, servers, network, applications, etc? A topic for more discussion another day.
