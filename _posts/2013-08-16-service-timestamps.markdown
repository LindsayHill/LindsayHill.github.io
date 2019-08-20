---
author: lindsay
date: 2013-08-16 19:45:45+00:00
layout: post
slug: service-timestamps
title: Service Timestamps - make your log timestamps readable
categories:
- NMS
tags:
- cisco
- Comware
- procurve
---

The default logging style for Cisco IOS-based devices is to insert the system uptime in the log entry. This makes it basically useless. Here's some typical log output:

```text
28w2d: %SYS-5-CONFIG_I: Configured from console by admin on vty1 (10.1.11.11)
```

This is the offending part of the configuration:

```text
service timestamps debug uptime
service timestamps log uptime
```

If a switch has been up for a few hours, then it's not too bad, as it will give you an indication to the nearest minute. But once a device has been up for more than a few days, it starts only telling you to the nearest day. After a year, it will tell you to the nearest week. So you can't tell if that port flapped 5 minutes or 5 days ago. Hopeless. You need to put these lines into your configuration:

```text
service timestamps debug datetime msec localtime
service timestamps log datetime msec localtime
```

Now your log entries will look like this:

```text
Aug  2 14:22:20.033: %SYS-5-CONFIG_I: Configured from console by admin on vty0 (10.1.11.11)
```

Isn't that so much better? If you haven't configured NTP correctly, you will see a `*` at the start of the line - this indicates that the time is not externally synchronised. Another option you might like to include is `show-timezone` which will add **NZST** to your logs. Quite useful if you work with devices using a variety of timezones.

HP Comware devices have some related options for displaying date/time with log entries - but luckily they take a more sane approach of defaulting to displaying date/time with the log. Here's some of the options:

```text
[Comware]info-center timestamp ?
  debugging  Set the time stamp type of the debug information
  log        Set the time stamp type of the log information
  loghost    Set the time stamp type of the information to loghost
  trap       Set the time stamp type of the alarm information
[Comware]info-center timestamp loghost ?
  date          Information time stamp of date type
  iso           Information time stamp of format in ISO 8601
  no-year-date  Information time stamp of date without year type
  none          None information time stamp
```

So you can choose what format to display the date in (or not to display it), and you can insert different timestamps in different log destinations. You might want to display timestamps on locally buffered logs, but remove the date from logs sent to the syslog server, since syslog servers will generally insert their own date. Typical output might look like this:

```text
%Jul 18 22:04:12:926 2013 COMWARE LLDP/6/LLDP_DELETE_NEIGHBOUR: -Slot=2; Neighbor deleted on Port GigabitEthernet2/0/15 (IfIndex 17825806), Chassis ID is 10.1.1.253, Port ID is 0060-b9ee-da45.
```

As for Procurve devices? Well, they don't offer granularity in log format. They do have an interesting option for viewing buffered logs though. Typical `show log` output will look like this:

```text
I 05/26/13 06:17:41 00636 ssh: sftp session from 10.1.11.11
W 05/26/13 21:02:09 03362 auth: User 'manager' login from 10.1.11.11
```

But if you enter `show log -b` it shows the timestamps as time since the switch rebooted:

```text
I 0000:00:06:00.56 00636 ssh: sftp session from 10.1.11.11
W 0000:00:06:06.64 03362 auth: User 'manager' login from 10.1.11.11
```

I don't really know why they offer that option, but didn't implement any other controls around syslog format. At least they default to a reasonable log display format.
