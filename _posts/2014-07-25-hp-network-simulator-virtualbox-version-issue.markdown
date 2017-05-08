---
author: lindsay
date: 2014-07-25 09:01:26+00:00
layout: post
slug: hp-network-simulator-virtualbox-version-issue
title: HP Network Simulator - VirtualBox Version Issue
categories:
- Routing &amp; Switching
tags:
- hp
---

HP has released an updated [Network Simulator](http://h20566.www2.hp.com/portal/site/hpsc/public/psi/home?sp4ts.oid=7107837&ac.admitted=1406274014017.876444892.199480143#downloadOptions). This uses [VirtualBox](http://www.virtualbox.org/) to provide a hypervisor, as opposed to [QEMU](http://www.qemu.org/) in earlier versions. When I tried it previously, it was [unusably slow]({% post_url 2014-02-24-hp-simware-comware-os-simulator %}). This was in part due to my using nested virtualisation.

The new version runs much better with nested virtualisation, although it's still not fantastic - I'd rather just spin up a [VSR1000]({% post_url 2013-10-14-hp-vsr1000-getting-started %}) system if I wanted to do something that didn't need testing switching code. That said, the Network Simulator does make it a lot simpler when you want to spin up several instances, and interconnect them in interesting ways.

There is one wrinkle though - the documentation states that it requires "Oracle VM VirtualBox Release 4.2.18 or later." At the time of writing, the current [VirtualBox version](https://www.virtualbox.org/wiki/Downloads) is 4.3.14. The problem is that if you install 4.3.14, and then try to install the Network Simulator, you'll get this pop-up:

[![VirtualBox error](/assets/2014/07/VirtualBox-error.png)](/assets/2014/07/VirtualBox-error.png)

That's right - the docs say 4.2.18 or later, but it seems that it won't install unless you install EXACTLY 4.2.18. I couldn't even get it working with later versions of 4.2.

Install 4.2.18, and then it allow you to install the HP Network Simulator. I'm sure there's no real reason for requiring 4.2.18, it's probably just an oversight, where the installer code is looking for a specific version, not a minimum version.

I have also tried installing VirtualBox 4.2.18, installing HP Network Simulator, then later upgrading VirtualBox. If you do that, you get this error:

[![VirtualBox not installed](/assets/2014/07/VirtualBox-not-installed.png)](/assets/2014/07/VirtualBox-not-installed.png)

I was able to go back to 4.2.18, then upgrade to 4.2.26 (the latest 4.2.x version currently available). That seems to launch and run OK. I'm not a big VirtualBox user, so I'm not worried about missing any 4.3 features. If you do figure out how to get 4.3 working with HP Network Simulator, let me know.
