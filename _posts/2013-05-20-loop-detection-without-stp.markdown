---
author: lindsay
date: 2013-05-20 17:00:52+00:00
layout: post
slug: loop-detection-without-stp
title: Loop detection - without STP
categories:
- Blog
tags:
- hp
- procurve
---

If you have a strong Cisco background, then you immediately think of Spanning Tree Protocol when you think of Layer 2 loop protection. Or if you're keeping abreast of the newest developments, you think of TRILL and SPB. But there are other mechanisms for helping detect loops at layer 2. Here's one I came across while studying for HP Master ASE: **HP Procurve Loop-Protection**.

According to the documentation:

> Loop protection provides protection against loops by transmitting loop protocol packets out of ports on which loop protection has been enabled. When the switch sends out a loop protocol packet and then receives the same packet on a port that has a receiver-action of send-disable configured, it shuts down the port from which the packet was sent.

So it's not doing anything fancy - just using its own protocol packet to detect loops back to the switch itself. Note this is different to Cisco's loopback detection, which will detect a switchport looped back on itself - e.g. due to faulty wiring - but will not detect a cable run from one port to another on the same switch. Configuration is quick and easy, and it seems to work well. It should work well in any situation where you have edge ports that users might connect together, possibly via a 'dumb' switch that doesn't forward STP BPDUs. These loop protection frames should be forwarded.

Let's take a quick look at the configuration and operation on my lab switch, an HP 2910al-24G Procurve switch, running W.15.08.0012. The default configuration is for loop-protection to be disabled:

```text
sw01pi# show loop-protect

 Status and Counters - Loop Protection Information

 Transmit Interval (sec)     : 5
 Port Disable Timer (sec)    : Disabled
 Loop Detected Trap          : Disabled


       Loop    Loop     Loop   Time Since  Rx           Port
  Port Protect Detected Count  Last Loop   Action       Status  
  ---- ------- -------- ------ ----------- ------------ --------
sw01pi#
```

In my lab, I've run a cable between ports 21 and 22, so we can simulate a loop. Currently both ports are shut down. Spanning-tree is disabled on this switch, so if I enable those ports, we've got a loop, and things will get messy. Let's configure loop protection, and then enable the ports:

```text
sw01pi# conf t
sw01pi(config)# loop-protect
 disable-timer         Specify the number of seconds to wait before re-enabling
                       a disabled port.The default is 0, which will prevent
                       automatically re-enabling the port
 [ethernet] PORT-LIST  Specify the ports that are to be added to/removed from
                       loop protection.
 transmit-interval     Set time between packet transmissions.
 trap                  Specify loop protection traps that are to be
                       enabled/disabled.
sw01pi(config)# loop-protect 21-22
sw01pi(config)# loop-protect trap loop-detected
sw01pi(config)# interface 21-22
sw01pi(eth-21-22)# enable
```

OK, let's take a look at what's happened with our ports:

```text
sw01pi# show loop-protect

 Status and Counters - Loop Protection Information

 Transmit Interval (sec)     : 5
 Port Disable Timer (sec)    : Disabled
 Loop Detected Trap          : Enabled

       Loop    Loop     Loop   Time Since  Rx           Port
  Port Protect Detected Count  Last Loop   Action       Status  
  ---- ------- -------- ------ ----------- ------------ --------
  21   Yes     No       0                  send-disable Down
  22   Yes     Yes      1      2s          send-disable Down
sw01pi#show log -r
---output truncated---
I 05/18/13 17:13:31 00077 ports: port 21 is now off-line
I 05/18/13 17:13:31 00077 ports: port 22 is now off-line
I 05/18/13 17:13:30 00898 ports: Loop Protect(62) has disabled port 22
I 05/18/13 17:13:30 00884 loop-protect: port 22 disabled - loop detected.
I 05/18/13 17:13:30 00076 ports: port 22 is now on-line
I 05/18/13 17:13:30 00076 ports: port 21 is now on-line
```

Pretty simple hey? One of the ports has been shut down, as expected. It has been logged, and an SNMP trap (`OID: 1.3.6.1.4.1.11.2.14.11.5.1.12.1.5.6.1`) has been generated. We've got the "Port Disable Timer" at the default of "Disabled" - this means that the port will stay shut down until we take manual action. We'd like it to re-enable after 10s, so we configure this:

```text
sw01pi(config)# loop-protect disable-timer 10
```

Now if we keep an eye on the output of `show loop-protect` we can see the Loop Count incrementing, as the port gets re-enabled, detects a loop, and shuts down again:

```text
sw01pi# show loop-protect

 Status and Counters - Loop Protection Information

 Transmit Interval (sec)     : 5
 Port Disable Timer (sec)    : 10
 Loop Detected Trap          : Enabled

       Loop    Loop     Loop   Time Since  Rx           Port
  Port Protect Detected Count  Last Loop   Action       Status  
  ---- ------- -------- ------ ----------- ------------ --------
  21   Yes     Yes      0                  send-disable Down
  22   Yes     No       5       6s         send-disable Down
sw01pi#
```

So now you can set a disable timer, and once users remove the loop, their network ports will come back up, without needing any input from the local network admin. All up, this is an easy feature to configure, and should be used on all edge ports on Procurve switches.

Comware has a very similar feature, called loopback-detection. Currently I don't have any Comware-based devices in my lab though...unless someone wants to donate some?
