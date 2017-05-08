---
author: lindsay
date: 2016-07-13 06:12:50+00:00
layout: post
slug: netmiko-support-for-brocade-icx-and-mlxe
title: netmiko support for Brocade ICX and MLXe
categories:
- NMS
tags:
- Brocade
---

netmiko is a "Multi-vendor library to simplify Paramiko SSH connections to network devices," written by [Kirk Byers](https://twitter.com/kirkbyers). It doesn't solve all of your pain with dealing with CLI-only network devices, but it tries to at least take away the low-level hassle of setting up a connection, and handling variations with things like enable mode, line-breaks, etc.

I've submitted a couple of PRs over the last few days to support Brocade ICX and MLXe devices - [#235](https://github.com/ktbyers/netmiko/pull/235), [#236](https://github.com/ktbyers/netmiko/pull/236) and [#237](https://github.com/ktbyers/netmiko/pull/237). These have now been merged into the master code.

This has not yet had extensive testing. Please try it out, and report any [issues](https://github.com/ktbyers/netmiko/issues).

I'm currently looking at VDX support. Looks like a few oddities around detecting the prompt, and dealing with the banner. Feel free to pitch in!
