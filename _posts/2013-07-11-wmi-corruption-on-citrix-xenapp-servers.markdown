---
author: lindsay
date: 2013-07-11 22:44:44+00:00
excerpt: The Windows WMI repository can become corrupted on Windows 2008 R2 servers
  being used with XenApp. WMI corruption can cause problems for monitoring systems,
  and potentially some applications. Here's how to fix the issue.
layout: post
slug: wmi-corruption-on-citrix-xenapp-servers
title: WMI Corruption on Citrix XenApp Servers
categories:
- NMS
tags:
- citrix
- wmi
---

The Windows WMI repository can become corrupted on Windows 2008 R2 servers being used with XenApp. WMI corruption can cause problems for monitoring systems, and potentially some applications. You can rebuild the repository, but that may lead to other issues, and it doesn't fix the underlying problem.

The problem occurs on systems that have many people logging into them, and hence a large amount of GPO logging. This logging affects the WMI repository, causing it to grow considerably, and sometimes get corrupted.

You can check to see if WMI is working properly by running `wbemtest.exe`. Click "Connect", leave everything at default, and hit Enter. Click Query, and enter `Select * from Win32_Process` Run that, and you should get a list of all processes running. If that doesn't work, you've probably got an issue with WMI. If you think it's caused by GPO logging, here's how to disable it:




  
  * Edit a GPO that gets applied to this server.

  
  * Go to Computer -> Administrative -> System -> Group Policy. Enable the setting "Turn off Resultant Set of Policy Logging."

  
  * Apply that, and you should get no corruption in future.



This won't fix any existing corruption though - I'd be inclined to just rebuild the server if you can, but an alternative is to rebuild the repository by stopping the WMI service, renaming `C:\Windows\System32\wbem\repository` to something else, and restarting WMI. You can also try running `winmgmt /repairrepository`. Note you can also check your repository state with `winmgmt /verifyrepository` This won't make any changes, but will tell you if there are any known issues.
