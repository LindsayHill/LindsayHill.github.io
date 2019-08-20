---
author: lindsay
date: 2015-05-21 05:42:19+00:00
layout: post
slug: using-check-point-identity-awareness-with-nat
title: Using Check Point Identity Awareness with NAT
categories:
- Security
tags:
- Check Point
- NAT
- security
---

Check Point [Identity Awareness](https://www.checkpoint.com/products/identity-awareness-software-blade/index.html) is problematic in environments that have multiple customers, overlapping private address space, and NAT. It can be done, if you understand the traffic flows, the connections needed, and how to combine several features. Here's how I did it.

{% include note.html content="NB: This post is not a full explanation of Check Point Identity Awareness, nor is it a discussion of the product design decisions, good or bad. It assumes that the reader understands what Identity Awareness is, and focuses on how to implement it when you also need to use NAT. It will be pretty dull reading to everyone else." %}

## Background: Typical Check Point Management Flows

A quick reminder of the traditional flows used for Check Point firewall management:

[![Check Point Management Flows](/assets/2015/05/Check-Point-Management-Flows.png)](/assets/2015/05/Check-Point-Management-Flows.png)Check Point Management Clients (e.g. SmartDashboard, SmartLog) connect to the management server to configure policies, view logs, etc.

Policies are compiled and pushed from the management server to the firewall(s). Logs are sent from the firewall back to the management server. All good.

## Identity Awareness: Additional Connections

Identity Awareness lets you define rules based upon user identities, rather than IP addresses. So you can say "This AD group is allowed to connect directly to the SQL Server." Much nicer than allocating static IPs to users.

Using Identity Awareness with AD Queries adds a few extra flows to the above picture:

[![Check Point IA Flows](/assets/2015/05/Check-Point-IA-Flows.png)](/assets/2015/05/Check-Point-IA-Flows.png)

There's three more connections we need to be aware of:

1. The management server connects to the Active Directory Domain Controller (DC) at initial setup, to discover the list of domain controllers. It also makes that connection if you fetch the LDAP SSL fingerprint.
2. The client PC running SmartDashboard makes an LDAP connection to the DC when you add new Access Roles. It needs to do this to get the current list of AD users, groups, etc. These connections are only made when you change access roles - this is not an ongoing connection. Note: The client will only connect to one DC.
3. The firewalls use WMI to connect to the DCs to read the Security Event Logs. They do this to build up a list of user-to-IP mappings. These connections are permanent. By default, all firewalls will connect to all defined DCs.

In simple environments, these connections are no big deal.

## Problem: MSPs

What happens if we're in a Managed Security Provider environment, where one Provider-1 server is used to manage multiple customers?

[![Check Point MSP Flows](/assets/2015/05/Check-Point-MSP-Flows.png)](/assets/2015/05/Check-Point-MSP-Flows.png)

I've only shown one firewall here, but we could have many customers managed by that same Provider-1 server. The firewalls used by each customer will have a consistent address scheme - either public IPs, or centrally allocated private ranges. NAT can also be used between Provider-1 and the managed firewalls. This is usually easy to manage.

Identity Awareness complicates this. Customers will almost certainly be using private IP addressing for their DCs, and there will be overlaps. Within each customer environment, the connections from the local firewall to the local DC are fine. The problem is for those connections needed from the Management Clients and the Management Server to the DCs. How can you route that? Won't the AD WMI connections get broken if you use NAT?

## Answer: Small doses of NAT

This is the outcome we're looking for:

1. Management Clients and the Management Server/CMA should use a NAT IP to connect to the Domain Controllers.
2. Firewalls should only connect to real IPs.

When we set up Identity Awareness, if we only use the real IPs, then the firewalls will connect to the DCs. But we won't be able to add new Access Roles, so it will be a bit pointless.

If we define routable NAT entries for the DCs, then the management connections will work. But the firewalls will either be unable to connect to those IPs, or they'll run into WMI challenges due to embedded IPs.

Here's the steps to get it working:

1. Set up a NAT IP for ONE of the DCs - e.g. DC1-XLATE. Configure firewall + NAT rules that allow the management clients & server to connect to that IP using LDAP + LDAPS.
2. Make sure your rulebase has objects for all of the DCs you want to use (e.g. DC1, DC2, DC3). Add firewall rules as needed for connectivity between firewalls and DCs.
3. Don't use the Identity Awareness Wizard. Manually add the LDAP Account Unit (AU).
4. When configuring the LDAP AU, add server entries for all of the DCs (e.g. DC1, DC2, DC3) **AND for the NAT IP**. So your list of servers might have DC1, DC2, DC3, DC1-XLATE. Yes, that's right - DC1 has both the real & the NAT IP. Set the priority for DC1-XLATE to 100, leave the others at default. When you add DC1-XLATE, you should configure encryption, and fetch the fingerprint. Don't bother trying to do it for the other DC objects.
5. On the Objects Management tab, pick DC1-XLATE from the drop-down list. Even though you've added multiple DCs, SmartDashboard & the management server will only connect to one server.
6. Save the LDAP AU.
7. Edit your firewall object. Configure IA as per normal. Then Other -> User Directory, change "Account Units Query / All" to "Selected Account Units list." Add your new LDAP AU. Uncheck "Use default priorities." Set the priority of the DC1-XLATE object to use 1001. (Anything >1000 tells this gateway to ignore that DC).
8. Do the same for any other firewalls you have set up for IA, and install policy

Result:

1. Management clients & servers use the NAT IP for querying AD.
2. The firewalls use only the real IPs to read Event Logs. They ignore the NAT IP. Check this with "adlog a dc" - you'll see that NAT IP listed as "ignored."

Overall this method seems to work well. It's a bit fiddly to set up, especially if you have many DCs, and many firewalls, but at least it can be done.
