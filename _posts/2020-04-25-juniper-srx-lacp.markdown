---
author: lindsay
date: 2020-04-25
layout: post
slug: juniper-srx-lacp
title: Juniper Branch SRX LACP Weirdness
categories:
- Routing &amp; Switching
tags:
- Juniper
---

[Juniper SRX 300 Series](https://www.juniper.net/us/en/products-services/security/srx-series/srx300/) firewalls may stop forwarding traffic in some situations. The firewall says it is forwarding the traffic, but it doesn't work. Monitoring traffic looks OK, ARP entries are present, but traffic never gets to the destination, until you clear ARP. Turns out the problem comes from using LACP with fast timers and active mode. Luckily the fix is simple.

## Alert: Firewall Offline

Here's the situation we saw: Our NMS reported a Juniper SRX320 offline. All other devices at the site were still working, but the firewall was unreachable. Traffic from the firewall to the NMS goes via the firewall's default gateway. Firewall A in this diagram was unreachable, but Firewall B was fine.

[![network_overview](/assets/2020/04/srx320_layout.jpg)](/assets/2020/04/srx320_layout.jpg)

OK, what's happening? Why is my firewall unreachable?

## Firewall says its fine?

Try to ping Firewall A, no response. From the default gateway, we can see an ARP entry for the firewall, but no response to ping. We can log in to Firewall B, and we see an ARP entry for Firewall A. Crucially: **we can ping Firewall A from Firewall B**. Hmmm. That's strange. Why can we ping it from one locally connected device but not another?

From Firewall B, we SSH across to Firewall A. Everything looks fine - it's up and running, it has not restarted, security policies are as expected, and it has a valid ARP entry for its default gateway. Why can't we ping it?

When we monitor traffic, the firewal says it is generating packets with the right L2 headers, and forwarding them out the right interface. We see regular ARP traffic. But we have no reachability. 

On a whim, we try clearing ARP on the firewall. Traffic starts working again. Check the ARP cache, it has the same entry it had before. It wasn't like it had an invalid entry before.

The firewall starts working properly again, and might be OK for hours, days or weeks before it fails again.

## What's Happening?

We investigated it quite deeply, including a very long debugging session with JTAC with a firewall that was in a known-bad state. But we couldn't work out why it wasn't forwarding packets properly. Everything looked correct. Changing software versions made no difference. The only common factor was that this problem was only seen on SRX300-series devices. Not on old SRX200-series, or bigger iron boxes. Every few days or weeks a firewall would go offline, and clearing ARP restored it.

## LACP Timers?

We use LACP on these firewalls, and we set them up as active mode, with fast timers. JTAC suggested we should change that to passive, with slow timers. They didn't think this was the cause of the problem, it's just something that they suggest as a general good practice. This may be officially documented somewhere, but I haven't come across it. 

I resisted making that change at first, as LACP didn't seem related. LACP was working, and we weren't seeing any issues with the LACP interface.

On a whim, I made the change in one place. Couldn't tell straight away if it solved anything, because the problem only occurred every few days or weeks. But after a week that system was still looking OK. So we rolled out the change everywhere else:

```text
set interfaces ae0 aggregate-ether-options lacp periodic slow
set interfaces ae0 aggregate-ether-options lacp passive
```

A week goes by with no outages. Then another week...and another. We cracked it at last. I don't know **why** this works, but it does.

Footnote: I missed a system when rolling out the change. 3 months later that firewall went offline. None of the others had broken in that period. Proves that this was the issue. I just wish I knew why.
