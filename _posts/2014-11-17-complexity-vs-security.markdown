---
author: lindsay
date: 2014-11-17 16:48:16+00:00
layout: post
slug: complexity-vs-security
title: Complexity vs Security
categories:
- Security
tags:
- complexity
- security
---

Many of the ‘security’ measures in our networks add complexity. That may be an acceptable tradeoff, if we make a meaningful difference to security. But often it feels like we just add complexity for no real benefit.

Here’s some examples of what I’m talking about:

* **Multiple Firewall Layers:** Many networks use multiple layers of firewalls. If you have a strong policy that says all traffic must go via a server within a DMZ, this makes sense. But often we end up with the same connections going through multiple firewalls. We end up configuring the same rules in multiple places. No security benefit, but increased chance of making mistakes, and added troubleshooting complexity.
* **Chained proxies:** It’s pretty common to use a proxy server, to enforce HR and security controls on what users browse. But some organisations have chained proxies, where an internal proxy server connects to an upstream proxy server to get Internet access. The upstream proxy doesn’t add anything from a policy or control perspective. It’s just another point to configure and troubleshoot.
* **NAT/Routing:** First let me be clear: NAT is not complete security in itself, but it can form a valid part of your overall network security policy. That said, we often over-complicate things. I’ve seen designs that involved multiple levels of source + destination NAT, to get traffic to route via a specific set of firewalls. Were there any additional controls or inspections applied to that traffic? No, of course not. It was just that it was the ‘correct’ firewall for that type of traffic.
* **Anti-Spoofing:** In general this is a good idea - a basic sanity check for source addresses. At its simplest, it should match your routing table - that’s what features like [Unicast RPF](http://www.cisco.com/web/about/security/intelligence/unicast-rpf.html) do. But some systems separate anti-spoofing from routing. Result: People forget to update anti-spoofing config when they change routing, which leads to re-work. Added security: None.
* **Pointless ‘Security’ Agents:** I’ve seen security agents on client PCs that attempt to detect and log/drop Denial of Service attacks. Think about that for a moment: What’s the point in wasting cycles detecting and dropping volumetric DoS attacks on a client PC? What are you going to do about it? It’s a client machine, it’s too late to drop the traffic at that point. Is this a realistic threat that you A) should worry about and B) can mitigate? No, of course not. And the only time I’ve seen that agent fire? It thought that inbound UDP/4500 traffic from the VPN concentrator was an attack. Yes, security systems fighting with each other.

Sometimes we really do need extra complexity. Sometimes it really does add a layer of security. But often it’s just a pain in the ass for no real benefit. Constantly re-evaluate your security measures, and ask if they’re helping or hindering.
