---
author: lindsay
date: 2013-08-02 02:46:06+00:00
layout: post
slug: imc-changing-definitions
title: IMC - Changing Predefined Device Model Definitions
categories:
- HP IMC
tags:
- imc
- SQL
---

IMC has a predefined list of over 6,000 devices. This list maps sysOIDs to devices, so when you add a new device to IMC, it will say "this is a Cisco Catalyst 3560 Switch", or a "HP MSM 760 Wireless Controller", etc. Here's a sample of what that list looks like:

[![Device Model List](/assets/2013/07/device_model.png)](/assets/2013/07/device_model.png)

{:.image-caption}
Device Model List

Note the "Type" column displays "System-Defined" for most entries. These are defined when you install IMC. If you add your own sysOID mapping, it will list it as "Custom-Defined.". You can change any settings for Custom-Defined sysOIDs. But what if you need to change a System-Defined entry?

It seems that you can go directly into the database, and update the display name. To do this, use your favourite DB management tool, and go into the `config_db` DB. Look for the `imc_config.tbl_dev_type` table. Find the row that has sysobject set to the sysOID you want to change. Update the `dev_type_name` field, and set it to whatever you like. Now IMC will display the updated name!

This will almost certainly be overwritten by the next update, and of course if you break it, you get to keep the pieces. But if it's important to you to change that display name, it's easy enough to do. One thing to be clear on: This only affects the display name - it does not affect any other IMC behaviour.
