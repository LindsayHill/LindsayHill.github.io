---
author: lindsay
date: 2016-01-15 05:16:27+00:00
layout: post
slug: modifying-packet-captures-with-tcprewrite
title: Modifying Packet Captures with tcprewrite
categories:
- NMS
tags:
- tcpdump
---

Recently I wanted to look at the structure of [sFlow](http://www.sflow.org) packets. Of course I can read the specs, but it's often easier to look at some real packets. So I set up a simple network, configured sFlow, created some traffic across the network, and used tcpdump to capture the sFlow packets.

Unfortunately I had a bit of a brain fade, and configured sFlow to use port 2055, not port 6343. So it looked like this:

```sh
vagrant@ubuntu:~$ tcpdump -r sflow.cap
reading from file sflow.cap, link-type EN10MB (Ethernet)
13:48:37.812602 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
13:48:57.813663 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
13:48:59.061629 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 232
13:49:17.806908 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
13:49:37.804433 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
13:49:57.806000 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
13:50:17.808959 IP 10.254.4.125.44695 > 10.254.4.170.2055: UDP, length 148
vagrant@ubuntu:~$
```

This is totally fine, but by default [Wireshark](http://www.wireshark.org/) won't decode this as sFlow. It appears like this:

[![Wireshark_UDP_2055](/assets/2016/01/Wireshark_UDP_2055.png)](/assets/2016/01/Wireshark_UDP_2055.png)

No big deal, I can right-click, go "Decode As...", set UDP Port to 2055, choose "sFlow" from the drop-down, and hit OK. Now it looks OK:

[![Wireshark_sFlow](/assets/2016/01/Wireshark_sFlow-e1452834876958.png)](/assets/2016/01/Wireshark_sFlow.png)

But I don't want to do that every time I open the file. More to the point, I don't want to have tell someone else to do that when opening the file. I wanted to change the packets to have a destination port of 6343, the commonly used sFlow port. Wireshark will default to decoding UDP/6343 packets as sFlow.

I'd torn down my lab my this stage, and couldn't be bothered re-configuring it & collecting new traffic. What I really wanted was to edit the existing packets to change the destination port. I found [Tcpreplay](http://tcpreplay.appneta.com) did exactly what I needed. Here's how I did it:

```sh
vagrant@ubuntu:~$ sudo apt-get install tcpreplay
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  tcpreplay
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/448 kB of archives.
After this operation, 1,032 kB of additional disk space will be used.
Selecting previously unselected package tcpreplay.
(Reading database ... 100006 files and directories currently installed.)
Preparing to unpack .../tcpreplay_3.4.4-2_amd64.deb ...
Unpacking tcpreplay (3.4.4-2) ...
Processing triggers for man-db (2.6.7.1-1ubuntu1) ...
Setting up tcpreplay (3.4.4-2) ...
vagrant@ubuntu:~$ tcprewrite --portmap=2055:6343 --in sflow.cap --outfile=sflow_6343.cap
vagrant@ubuntu:~$ tcpdump -r sflow_6343.cap
reading from file sflow_6343.cap, link-type EN10MB (Ethernet)
13:48:37.812602 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
13:48:57.813663 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
13:48:59.061629 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 232
13:49:17.806908 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
13:49:37.804433 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
13:49:57.806000 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
13:50:17.808959 IP 10.254.4.125.44695 > 10.254.4.170.6343: sFlowv5, IPv4 agent 10.254.4.125, agent-id 0, length 148
vagrant@ubuntu:~$
```

Easy as that. Of course there are many more options for rewriting packets if you want to do something complex. All I needed was this simple change, and it was quick and easy.
