---
author: lindsay
date: 2013-04-23 19:00:36+00:00
layout: post
slug: imc-adapters-mikrotik-netscaler
title: IMC - Device Adapters for Mikrotik and Netscaler
categories:
- HP IMC
tags:
- custom adapters
- imc
- juniper
- mikrotik
- netscaler
---

I've written basic versions of Device Adapters for IMC, to allow configuration backup of Miktrotik RouterOS-based systems, and Citrix Netscaler-based systems. I've also posted a modified Juniper adapter, with a couple of modifications so it now works with SRX-110 devices.

These are both fairly rough, but they do the job. For the Miktrotik adapter, it uses the "export" command to grab the backup. This only works via CLI. The output needs some cleanup - there is some extra stuff left in there, and I'd like to re-join the long lines that get split up. Any volunteers? Note that this was tested out using the `cx` option - add that to the username when logging in, and the Mikrotik disables colour output.

The Netscaler adapter only works via CLI. I would like to find a way to save the running configuration as a separate file, so I could then retrieve both running and saved configurations via SCP or SFTP. If I retrieved the saved configuration using SFTP, it failed to pull the running config via CLI. It works if you only use CLI, for both startup and running. Again, the output needs a little bit of a tidy-up. The most useful thing would be to remove the date from the running configuration, as it triggers false positives for backup change alarms.

These have been uploaded to the GitHub repository we're using for NetOps: [github.com/NetOpsCommunity/imc-netops](https://github.com/NetOpsCommunity/imc-netops). If you're using IMC and either Netscaler or RouterOS devices, please try these out. There is more work to be done, but they will at least give you a basic backup of your configuration.
