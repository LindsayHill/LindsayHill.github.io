---
author: lindsay
date: 2013-03-10 02:01:49+00:00
layout: post
slug: imc-backups-telnet
title: IMC - Backups not working via Telnet?
categories:
- HP IMC
tags:
- hp
- imc
- NMS
---

Recently I was involved in a discussion on [HP's IMC Forum](https://community.hpe.com/t5/IMC/bd-p/network-management-a-series-forum), where MohammadH [needed help with IMC](https://community.hpe.com/t5/IMC/Help-with-IMC/m-p/5952971). One of the problems he had was switch backups not working properly. It can be a bit of a chore tracking down the source of the problem, but the best place to start is usually `<IMC install dir>\server\conf\log\imccfgbakdm.<current_date>.log.`

This logfile is a bit tricky to read, but there is a huge amount of information on the processes that IMC has gone through when trying to back up a device. In this case, we came across these lines in the logfile:


```text
2013-02-04 08:14:10.344 [INFO (0)] [THREAD(2580)] [CTclExecutor::exec_impl()] Begin to exec: spawn telnet 192.168.0.17
2013-02-04 08:14:10.361 [ERROR (1)] [THREAD(2580)] [CTclExecutor::exec_impl()] Failed to exec cmd: spawn telnet 192.168.0.17, error message: The system cannot find the file specified.
```


This is a problem that I've come across a couple of times, where IMC can't find the `telnet.exe` binary it uses for device backup. IMC should use its own copy, located at `<IMC install dir>\server\bin\`. But what I've seen is that if Telnet Client is disabled on Windows 2008 R2 (as it is by default), then IMC never installs its own copy of `telnet.exe`. Enabling the Windows Telnet Client, and manually copying that binary over doesn't work either - you must use the version that IMC expects.

The easiest way to find it is to use the original IMC installation files. Look in `windows\install\components\common\server\commonserver_win.zip`. Extract `telnet.exe`, and copy it to `server\bin\`. Backups should now work.

Note: If you use SSH or SNMP read-write, you probably won't notice this issue.

For a more in-depth look at tracking down problems with a device backup script, and how to fix it, check out my Packetpushers post - "[HP IMC: Backup Cisco ASAs via SSH](http://packetpushers.net/hp-imc-backup-cisco-asas-via-ssh/)"
