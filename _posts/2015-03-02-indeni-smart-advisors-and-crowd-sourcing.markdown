---
author: lindsay
date: 2015-03-02 20:00:44+00:00
layout: post
slug: indeni-smart-advisors-and-crowd-sourcing
title: Indeni, Smart Advisors and Crowd-Sourcing
categories:
- NMS
tags:
- indeni
- monitoring
---

Monitoring needs to move on from traditional fault and performance polling. It should include identifying common misconfigurations and known faults. We're all using the same technologies, so we've all got the same problems. I like the look of Indeni, a new approach to this problem. It uses a form of crowd-sourcing to act as a smart advisor.



## Precious Snowflakes?



We all think we’re precious snowflakes. But we’re not. We use the same technologies, glued together in the same ways. That means we all have the same problems, and make the same misconfigurations.

Vendors frequently publish new bug fixes, KB articles, EOS notices, etc. Some of these apply to products/versions/features we’re using. We struggle to keep up with the volume, and we miss these - so maybe our network is running with a known issue. Striking an unknown bug is bad. Getting caught out by a published issue is worse. Having an outage because we didn’t make sure the routing tables were in sync on our firewall cluster is unforgivable.



## Vendors Need Help Too



Information flow is a two-way problem. The vendors can’t always see how customers deploy their products in the real world. They think they know. They write manuals, they write white papers. But they don't know for sure what we do, and how their systems perform. They lack the hard data.

They can’t see problems such as slow memory leaks, because their tests don’t have the right traffic mix, or don't run long enough.

We need to be able to A) feed anonymous data back to the vendors, and B) get recommendations on how to make our network run better.



## Monitoring to the rescue?



In theory, current monitoring tools should be able to help us. They’re collecting state information from our network, and now they’re also collecting configuration information. But they have focused on fault detection, not configuration issues, or software faults.

Companies such as [Solarwinds](http://www.solarwinds.com/) have been taking steps to improve this. [NPM 11.5](https://thwack.solarwinds.com/community/solarwinds-community/product-blog/blog/2015/02/24/npm-115-now-available) goes to greater lengths to identify duplex mismatches in your network. [NCM](http://www.solarwinds.com/network-configuration-manager.aspx) helps with device EOL management, and can compare configurations against PCI-DSS recommendations.

But it’s still not enough. So we end up building up our own repositories of scripts, tips & tricks. In the early 2000s I wrote scripts to check firewall clusters, to make sure the routing & connection tables were in sync. Because I couldn’t take those scripts with me between contracts, I ended up re-writing them several times. They were useful scripts, because they always found problems. But where was the real value in me having to re-write those?

I would periodically check the vendor KBs for new articles, and skim over them to see if they applied to me. I read release notes, and checked end of life notices. But that all relied on me. What happened when I left? Most of that stopped happening. What about new admins that haven’t yet figured out what they need to do?

Traditional monitoring tools aren’t helping us with those problems. And they’re not helping the vendors either, because the info only goes one way. There is some hope with the cloud-managed platforms, such as [Meraki](http://meraki.cisco.com/). They’ve got a complete view of how customers use Meraki products, and how they’re performing. But most vendors don’t.



## Indeni - a Trusted Advisor?



That’s why I like [Indeni](http://www.indeni.com)’s approach. Indeni is a sort of trusted advisor for Check Point, F5 & Cisco systems.  It connects to your systems, grabs the configuration and state, and compares this against a list of known issues. It’s a form of monitoring tool, although it does not replace a traditional NMS.

The most interesting part of what it does is that the list is not static. It is constantly updated, in response to vendor advisories, customer-reported issues, etc. They can also feed information back to the vendors - e.g. if there is a jump in memory usage on HFA 20 systems, maybe there's a problem. It’s a form of crowd-sourcing.

If you’ve ever worked with Check Point products, you’ll understand why Indeni makes sense. Check Point separates firewalling from the underlying OS. This means many management ‘points’ -  firewall policy, global settings, *.def files, OS configurations, etc. There’s a huge number of different knobs to turn, and they’re not always easy to find. It’s not like an ASA, where you can define your whole configuration in one text file, with an associated binary blob of firmware.

Check Point can have driver issues, NTP misconfiguration, kernel parameter changes, routing issues, etc. That's all before you’ve even touched the actual firewall policy. Some things you’ll only find out the hard way. Check Point has had many 10Gb NIC driver issues, but you won’t find out until you log a support case, and they issue you a special hotfix.

Anyone supporting a large Check Point network will have their own scripts, tweaks, policies, etc. But it’s still quite reactive, and it doesn’t scale down to smaller networks. It’s also difficult to support when personnel change. Check Point appears to have little interest in fixing the underlying issues, so we need to use something like Indeni.

You could apply this sort of model to other products that are widely deployed - e.g. Ubuntu, Apache, Palo Alto Firewalls, etc. There are still trust issues to work through though. You want to know what information gets shared, and with whom. The vendors also need to know that their product & customer information is not shared with other vendors.

What do you think? Do you see value in having a tool that checks your system configurations, and makes recommendations? What about the crowd-sourcing aspects of it? Does that excite you or scare you?
