---
author: lindsay
date: 2014-01-25 20:00:35+00:00
layout: post
slug: nxlog-convert-to-syslog
title: nxlog - Convert any text file to Syslog
categories:
- NMS
tags:
- logs
---

Recently I've been converting a network from an agent-based monitoring system to an agentless system. One challenge was handling custom application logfiles. Most agent-based monitoring systems make it very easy to add logfile encapsulators, to read a custom file, and react to specific patterns. But now I needed to convert the logfile into syslog, so it could be directed to my NMS, which would parse the syslogs, and raise alerts as required.

On Linux systems, there are many options for converting arbitrary logfiles into syslog. For Microsoft Windows, there are reasonable free options for converting Eventlogs to Syslog, such as [SNARE](http://sourceforge.net/projects/snare/). There are excellent, but expensive tools like [Splunk](http://www.splunk.com/) for parsing any sort of data you like. But I really struggled to find anything that could run on Microsoft Windows, convert a custom logfile to syslog, and send it to two destinations simultaneously.

After much fruitless searching, and more than a few dead-ends, I found the answer: [nxlog](http://nxlog-ce.sourceforge.net/).

> It can collect logs from files in various formats, receive logs from the network remotely over UDP, TCP or TLS/SSL on all supported platforms. It supports platform specific sources such as the Windows Eventlog, Linux kernel logs, Android device logs, local syslog etc. Writing and reading logs to/from databases is also supported for many database servers. The collected logs can be stored into files, databases or forwarded to a remote log server using various protocols.

That does pretty much everything I want - many, many options for both input and output, and I can parse the content too.

Installation was pretty simple. Install the MSI package, and then edit the configuration file at `C:\Program Files (x86)\nxlog\conf\nxlog.conf`. There's three key sections - **Input**, where you define the input data sources, **Output**, where you define the possible output methods/destinations, and **Route**, where you define the mapping between inputs and outputs.

```xml
define ROOT C:\Program Files (x86)\nxlog
Moduledir %ROOT%\modules
CacheDir %ROOT%\data
Pidfile %ROOT%\data\nxlog.pid
SpoolDir %ROOT%\data
LogFile %ROOT%\data\nxlog.log

<Extension syslog>
 Module xm_syslog
</Extension>

<Input in>
 Module im_file
 File 'c:\MyApp\logs\applog.log'
 SavePos TRUE
 ReadFromLast TRUE
 PollInterval 1
 Exec $Message = $raw_event; $SyslogFacilityValue = 22;
</Input>

<Output out1>
 Module om_udp
 Host 10.1.1.1
 Port 514
 Exec to_syslog_bsd();
</Output>

<Output out2>
 Module om_udp
 Host 10.1.1.2
 Port 514
 Exec to_syslog_bsd();
</Output>

<Route 1>
 Path in => out1, out2
</Route>
```

That's reasonably easy to follow - you can see we set some global settings, load some required modules, then define the input type, location and handling. You can specify how often to read the file, whether to re-read on startup, etc. You can set syslog facilities and values. The Output sections then define each of my syslog servers. Finally the routing section maps from **in** to both of the **out** locations. You could have separate handling for various input files, Eventlogs, etc. You can also add all kinds of complex pattern matching, parsing, external programs, etc.

My configuration is very simple, and barely scratches the surface of this powerful tool. Check out the rest of the [documentation](http://nxlog-ce.sourceforge.net/nxlog-docs/en/nxlog-reference-manual.html) for more ideas on data manipulation, parsing and routing. Highly recommended.
