---
author: lindsay
date: 2015-03-28 21:00:13+00:00
layout: post
slug: check-point-upgrade-without-dropping-connections
title: Check Point - Upgrade Without Dropping Connections
categories:
- Security
tags:
- Check Point
---

Check Point firewall upgrades have always been painful. The loss of connection state is a big part of this. Existing connections stop working, and many applications need restart. It looks like there is a way of minimising this pain on upgrade.

Stateful firewalls record the current 'state' of traffic passing through, so they can recognise and allow reply or related traffic. If you have a firewall cluster, they need to synchronise state between the cluster members. This is so that if there is a failover, the new Active node will be aware of all connections currently in flight.

If you have a failover, and the standby member is NOT aware of current connection state, it will drop all currently open sessions. Any packet that isn't a SYN packet will get dropped, and the applications need to establish new connections. Some applications handle this well - especially those that use many short-lived connections such as HTTP or DNS. But other applications that have long-running connections - e.g. DB connections - may struggle with this. They think the connection is still open, and take a long time to figure out it's broken. They may eventually recover on their own, or they may need manual intervention.

Check Point clusters use dedicated interfaces for state synchronisation. When a firewall first starts up, it goes through a full synchronisation process, getting the complete session table. After that, only deltas are synchronised - i.e. new connections, and changes of state for existing connections.

{% include note.html content="Aside: These sync processes use different protocols. I have seen situations where firewall policy did not allow FW1 between cluster members. As a result, initial sync didn't work, but delta sync did. You end up with a different number of connections on each member - e.g. 10,000 on the active node, and 5,000 on the standby. Eventually they'll get synchronised, once every application has restarted. This could take weeks or months." %}


The above is all well & good, and it mostly works. I'm surprised it still needs so much care & attention in 2015, but hey, this is Check Point we're talking about.



## What happens when you do an upgrade?



This is where problems start. Check Point has historically not liked doing state synchronisation between different software versions. So if you're running NG FP2, and you upgrade one node to NG FP3, it won't synchronise with the other node. I can sort of understand this, because the data structures may well change between versions. Usually (but not always) you can do state sync with minor upgrades, e.g. for hot fixes.

When you promote the upgraded node to "Active" it will silently drop all current connections. This makes upgrades quite painful, as you are potentially breaking all applications that traverse this firewall. You've got enough to worry about with Check Point changing inspection behaviour between versions. Now you need all your application teams to be on standby to test and maybe restart their apps. Major pain. Major hassles getting your change approved. It's probably the biggest thing holding back regular upgrades.

I know, you're thinking that your hip Web 3.11 application can gracefully handle these sorts of failures. Over here in the Real World, crapplications just can't handle it. Sorry, but that's what we have to deal with. These are the same applications that assume they can open a DB connection, pass no traffic for 2 months, then assume they can re-use that established connection.



## What can we do about it?



Check Point has been deaf to this for a long time. They seem to just ignore it as an issue. This is a classic example of what happens when you're too far removed from real-world operations.

Check Point has recently offered a non-standard patch that will let you synchronise **new** connections between different versions. So delta sync will work, but that initial sync of all current connections doesn't. Result: all your short-lived connections that you didn't really care about get synced, but the long-running DB connections don't. Completely missing the mark there Check Point.



## Maybe there is an answer



The problem is that the firewall will drop those old connections as "Out of State." The default setting is to drop Out of State traffic. But we can change that. What if we allowed Out of State traffic, at least for a while?

I've always assumed this will work, but wasn't sure. Traffic will still be checked against the security policy, but it won't care about the TCP state. So in theory, all our existing sessions will be allowed through.

But we don't want to allow out of state forever. Otherwise we might as well just have a router with ACLs. What happens when we re-enable state enforcement? My concern was that the firewall would not 'learn' those connections that it had seen as out of state.

I'm pleased to report that it does indeed 'learn' about those connections. My colleague has been doing some testing, and it looks like it all works. Existing connections will added to the connection table, and once you re-renable state enforcement, those sessions continue to work.

Now your upgrade process can look like this:




    
  1. Upgrade the passive node

    
  2. Install policy that allows Out of State traffic on that cluster. Install to upgraded member

    
  3. Promote the passive node to active

    
  4. All sessions will now go via the upgraded node. It will allow existing sessions, and learn about new sessions. Leave it running like this for a while - say 24h.

    
  5. Re-enable state enforcement. Any connections that have done ANY traffic in the last 24 hours will have been learnt, and will be allowed. Anything that didn't send any traffic for 24 hours deserves to get dropped.



You should still do your own testing for this. But it looks like we've finally validated an upgrade path that will reduce your pain.
