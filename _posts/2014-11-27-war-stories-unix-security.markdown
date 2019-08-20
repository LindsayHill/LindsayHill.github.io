---
author: lindsay
date: 2014-11-27 05:27:38+00:00
layout: post
slug: war-stories-unix-security
title: 'War Stories: Unix Security'
categories:
- Security
series: 'War Stories'
tags:
- war stories
---

{% include series.html %}

A different kind of war story this time: Unix security blunders. Old-school Unix-types will mutter about how much more secure Unix systems are than Windows, but that glosses over a lot. In a former life I worked as an HP-UX sysadmin, and I saw some shocking default configurations. I liked HP-UX - so much better laid out than Solaris - but it was very insecure by default. Here’s a few things I’ve come across:

## Gaining Root

We’d lost the root password for a test HP-UX server. We had user access, but not root. The server was located in a different DC, and we didn’t really feel like going and plugging in a console cable to reset the root password. So we started looking around at how we might get access. After a while I found these two things:

1. root’s home directory was `/` - this was the default on HP-UX
2. The [Remote Login](https://en.wikipedia.org/wiki/Rlogin) service was running

And now for the kicker:

```bash
hpux lhill$ ls -ld /
drwxrwxrwx 30 root wheel 1020 1 Nov 13:57 /
```

Put those together, and you can see it’s easy to gain root. All we needed to do was create [/.rhosts](https://docs.oracle.com/cd/E19455-01/805-7229/remotehowtoaccess-3/index.html), and add whatever we wanted. We set it up to allow root login from another server, and bingo - remote access as root. Saved a trip out to the DC.

## World-writable directories in $PATH

Another classic mistake was the permissions for `/usr/local/bin`:

```bash
hpux lhill$ ls -ld /usr/local/bin
drwxrwxrwx 30 root wheel 1020 1 Nov 13:57 /usr/local/bin
```

Can you see the problem here? Yup - any user can add executable they like to `/usr/local/bin`, and then you just need to trick someone into running it. A classic typo is to run `mroe` instead of `more` - so what would happen if we put a script called `mroe` into `/usr/local/bin`? We could put whatever commands we wanted in there.

```bash
#!/bin/sh
if [ $(whoami) -eq "root" ]
 then
 # Do_bad_stuff
 fi
 echo
echo "-sh: $0: command not found" >&2
exit 127
```

The malicious commands might involve creating a new user, with UID 0. Now all we have to do is wait for an admin to make a typo, and we’ve got root access.

## Chargen vs Echo

HP-UX shipped with both chargen and echo enabled by default. One service that responds to connections with a random character, the other one echoes back whatever it receives. So think about what happens if you fake a packet with a source of chargen and a destination of echo? Fun times. Lucky that one’s easy to see with an [IPS](http://tools.cisco.com/security/center/viewIpsSignature.x?signatureId=4061&signatureSubId=0&softwareVersion=5.1&releaseVersion=S31). Sure, things aren’t perfect with other Operating Systems. But next time someone starts going on about how great Unix security is, just remember that it wasn’t always true.
