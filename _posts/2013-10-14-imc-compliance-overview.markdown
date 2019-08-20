---
author: lindsay
date: 2013-10-14 12:00:45+00:00
layout: post
slug: imc-compliance-overview
title: IMC Compliance - Overview
categories:
- HP IMC
series: 'IMC Compliance'
tags:
- Compliance
- imc
---

{% include series.html %}

HP added the “Compliance Center” to HP IMC in early 2012. This series of posts will explore what it is and why it’s so useful, and will walk through both basic and advanced usage scenarios.

According to the fine manual…

> The Compliance Center feature supports organizations in adhering to compliance and standards…Compliance check verifies whether configurations on devices are compliant with compliance policies and shows the check results in various forms. In addition, compliance check can fix incompliant(sic) configurations to make sure the network operates in a secure, stable environment.

Let’s quickly summarise the capabilities of IMC Compliance Center, where and why would use it, and some terminology.

## Capabilities

Key capabilities of IMC Compliance Center are:

* **Configuration Backup Checks:** Run checks against the latest saved running or startup configurations
* **Active Commands:** Run active commands against devices and check the output.
* **Basic or Complex Pattern Matching:** Write rules that use simple pattern-matching, or full regular expressions.
* **Remediation:** Semi-automated remediation of non-compliant systems.
* **Scheduling:** Schedule checks to run on a periodic basis
* **Vendor/Device Targeting:** Write rules that target specific vendors and/or models.

## Why/where would I use it?

All well-managed networks will find a need for Compliance Center. No matter how well you think you execute your configuration changes, it’s always good to have automated checking in place, to ensure that the security and functionality of the network is not compromised. Sure, you install all new systems according to the templates - but can you be sure that every device that has ever been installed over the life of the network is properly maintained?

These are typical use-cases:

* **Security Compliance:** Define policies that check your configurations to ensure they meet your security policies. Run those checks against all your systems, so you can verify your security posture. This is particularly good for those who have to comply with specific standards such as PCI-DSS.
* **Auditing:** No-one likes being audited. All you want to do is get the auditor out of there as quickly as possible. If you can show that you are running automated checks against your configurations, then all you need to do is show the auditors your rulesets and compliance check history, confirm it’s working on a couple of devices, and the auditor will be happy and on their way. Otherwise you’ll find yourself having to go through every single configuration, line by line. Or worse, the auditor will make you enforce a regular manual compliance checking policy. No fun at all.
* **Functionality Checking:** Compliance isn’t just about security - you might want to check that all switches have DHCP snooping and BPDU-guard enabled, or maybe you need to check that your AAA configuration is the same everywhere. Easily done with Compliance Center.

## Terminology

Before diving too far into using IMC’s Compliance Center, we should go over a few of the terms used. These are some of the terms you’ll come across:

* **Check Task:** This is a “task” that tests Compliance Policies against either specific devices, or device models. Tasks can be run as immediate, one-off tasks, or they can be scheduled, for regular compliance checking.
* **Display Command:** These are specific commands to be executed on devices. IMC will login to the device, capture the output, then run compliance rules against this output.
* **Compliance Policy:** A set of Rules, grouped together in any way you see fit. IMC includes many pre-defined policies and rules. You can define your own, and/or re-use the predefined rules and policies. Note that Check Tasks are defined using Compliance Policies, not individual rules.
* **Rule:** The actual check to run. Rules will define specific patterns to search for, using either simple pattern matching or complex regular expressions. Searches can be positive or negative. Rules have target vendors, and/or specific devices. If you define a rule for Cisco devices, and try to run it against HP systems, it will be skipped as not applicable.
* **Check Type:** This lets you narrow the scope of the rule - the default is to run pattern matching against the entire configuration, but you can narrow it down to a specific interface, or range of interfaces. Or maybe you’d like to look in the “router ospf” section of a Cisco configuration.
* **Check Target:** Rules can be applied to either the last retrieved running configuration, the last retrieved startup configuration, or the output of a Display Command. Generally you want to run your checks against the latest running configuration, so you must ensure you have regular backups configured and working first!
* **Device Series:** Rules can have target Device Series they apply to. Device Series are groups of devices - e.g. All Cisco 3560 variants.
