---
author: lindsay
date: 2014-08-31 04:10:31+00:00
layout: post
slug: rant-just-stop-it-with-the-tftp
title: 'Rant: Just stop it with the TFTP'
categories:
- Routing &amp; Switching
tags:
- TFTP
---

[TFTP](http://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol) was first defined in 1980. That is a very long time ago in IT, and while it’s had a good run, it’s time for network engineers to stop using TFTP. It’s slow, insecure, and there are better options available.

TFTP is an unauthenticated, plain-text file transfer protocol. It is commonly used by network engineers to transfer switch configs, or IOS images. No passwords required, just a straight “Get this file ” or “Put this file ”. It uses UDP to transfer data. It is designed to be very simple, and light-weight. This is a large part of why it was popular - TFTP servers or clients could be implemented in low-powered devices, such as switches, VoIP phones, etc. Some systems also use it as part of an initial boot, where TFTP is used to retrieve the initial boot environment.

The main complaints I hear from engineers are “How do I get a TFTP server set up?”, and “Why is this taking so long to transfer?” Server configuration is just a Google exercise, but let’s look at file transfer speed.

## Speedy? Not so much

For this test, I have a CentOS 6.x VM running on my laptop. I’m downloading a 20MB file from the VM to my laptop.

```bash
lhill$ time scp centos6:/var/lib/tftpboot/testfile .
testfile 100% 20MB 20.0MB/s 00:01

real 0m0.827s
user 0m0.237s
sys 0m0.132s
lhill$ tftp 192.168.225.133
tftp> get testfile
Received 21135236 bytes in 9.1 seconds
tftp> quit
lhill$

```

That’s a pretty significant difference. If you’re transferring a large file across a network, this will mean the difference between waiting a few seconds, and wandering off to get a cup of coffee.

## But why is TFTP so slow?

I’ve taken a couple of packet captures here. This one shows a regular TFTP transfer. Click on the image to go to the Cloudshark-hosted version.

[![TFTP Transfer - click for Cloudshark version](/assets/2014/08/TFTP-Transfer-Truncated.png)](https://www.cloudshark.org/captures/067ee3e434bb)

{:.image-caption}
TFTP Transfer - click for Cloudshark version

Note how it’s only using a 512 byte block size. Each packet has to be acknowledged, making this a very slow method of transferring a large file. On a low-latency link, it makes a huge difference.

Look at this SCP session - again, click the image to go to the Cloudshark-hosted capture.

[![SCP Transfer - click for Cloudshark version](/assets/2014/08/SCP-Transfer-Truncated.png)](https://www.cloudshark.org/captures/9d3b99c0eb65)

{:.image-caption}
SCP Transfer - click for Cloudshark version

Note how TCP is doing its thing, and figuring out the optimum transfer window, and does not require acknowledgement of each packet before sending the next.

## What About Using Larger Block Sizes?

Some (most?) TFTP clients have an option to use larger block sizes. Most modern TFTP servers should support this. Use the `-e` option with the OS X TFTP client to set the block size to 65,536. Now look at the transfer times:

```bash
lhill$ tftp -e 192.168.225.133
tftp> get testfile
Received 20971520 bytes in 0.7 seconds
tftp> quit
lhill$
```

That’s a lot better. Look at the packet capture, and you can see more of what’s going on - the TFTP server sends many fragmented packets, which get re-assembled by the client.

[![TFTP transfer using larger block size - click for Cloudshark version](/assets/2014/08/TFTP-Extended.png)](https://www.cloudshark.org/captures/17967fd28649)

{:.image-caption}
TFTP transfer using larger block size - click for Cloudshark version

## Still Not Good Enough

So we can improve the transfer rate somewhat. But ask yourself what would have happened if we’d lost some packets in the above trace? It would be handled poorly, since the client/server can’t dynamically adjust to changing conditions.

If you’re transferring a large IOS image (or worse, a full IOS-XR image), these small differences can make difference of tens of minutes, or even hours in transfer time.

What about text files though? If we’re only transferring a text configuration of a couple of kB, the speed doesn’t really matter much. But do you really think it’s a good idea for your network configurations to be flying around the network in plaintext? How do you know they weren’t intercepted or manipulated?

And the TFTP server itself has no security. How do you know that the config was uploaded from your device? What’s stopping someone else retrieving all your configs? Or uploading malicious configuration?

## Change your methods

What’s stopping us using better methods (SCP/SFTP, or at a pinch, FTP) ? Mainly inertia, and old habits. There are definitely systems that will only use TFTP, but there’s many that no longer require it - e.g. VoIP phones might need to get their initial configuration via TFTP, but they can probably download their firmware via HTTP these days. I’m sure you’ve got a special use-case, but make sure you’ve actually checked the docs first.

Do yourself a favour: Next time you think you need to use TFTP to transfer a file, ask yourself: What other mechanisms exist to do this? Is TFTP my only possible option? Save yourself the time and hassle, and maybe make your network a little more secure.
