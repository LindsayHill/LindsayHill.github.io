---
author: lindsay
date: 2016-03-10 05:35:48+00:00
layout: post
slug: war-stories-backup-nics-dns-and-ad
title: 'War Stories: Backup NICs, DNS and AD'
categories:
- Worklife
series: 'War Stories'
tags:
- war stories
- worklife
---

{% include series.html %}

A return to our sporadic series of networking war stories. This time it’s fun with dedicated backup networks, DNS auto-registration, and Active Directory. Thank God it’s a lot easier these days with virtualisation. But back then…


## Backups suck, but you need to do them somehow


Back in the olden days we had a dedicated tape drive connected to each server. Daily/weekly backups were written to the local tape drive using a SCSI connection. Someone would walk around the servers each day and change the tapes. It was simple, and it worked, but it doesn’t scale.

Two things happened - server numbers started exploding, and Gigabit Ethernet became practical. That meant that it became practical to have centralised ‘backup’ servers connected to tape drives, and to stream backup data across the network. Much better scale - we only needed to install an agent on each server, and the centralised backup servers needed to have enough tapes + tape drives. This also gave us much better central control & visibility of our backups.

Of course, we were worried about the impact of streaming large backup files across the network. We didn’t want that to affect production traffic, so we installed dedicated backup NICs on each server. Yes, that did increase cost, but it was still cheaper & easier than hundreds of tape drives.

Very simplified, our network looked like this:

[![Backup Network](/assets/2016/03/Backup-Network.png)](/assets/2016/03/Backup-Network.png)

We put the backup server in the same subnet, so traffic was directly switched, without traversing any firewalls or routers. Note that this also meant that we didn’t need any custom routing on each server. They would keep using their default gateway for regular traffic, and use the locally connected route for backup traffic.


## Umm, what just happened to my application?


Initial testing looked good. We added the backup NIC, and tested backups. Everything looked good. Traffic was routing as expected - data traffic went via the firewall, backup traffic used the local connection to switch traffic to the backup server via the backup NIC. OK, let’s start rolling it out to more systems.

And…some applications start breaking. Huh? What’s going on? Applications are saying that connections are timing out. But hang on, I can RDP to that server. It’s up & running. Firewall rules haven’t changed. What the hell is going on?

Looking through the logs a bit more, I notice something strange. Clients are trying to connect to 20.20.20.x addresses - ie the backup network. Of course the firewall drops that traffic. But why would a client try to connect to that network? How does it even know that network exists?


## What’s going on?


By default, Windows systems attempt to dynamically register their hostname in DNS. This is configured on the network adapter. On IPv4 Properties, go to Advanced, DNS tab:

[![DNS Registration](/assets/2016/03/DNS-Registration.png)](/assets/2016/03/DNS-Registration.png)

The online help explains what this option does:


> Register this connection’s addresses in DNS specifies that the computer attempt dynamic registration of the IP addresses (through DNS) of this connection with the full computer name of this computer, as specified on the Computer Name tab (available in System in Control Panel).


So our problem was that when we added a secondary interface, it registered a new IP in DNS. AD DNS now had two A records for that hostname. You had a 50/50 chance of being handed out the ‘wrong’ IP address from DNS. Applications that only used IP addresses were OK, but applications using hostnames could break. It could be hard to predict, as an application could cache the results of the DNS lookup.

Good thing we were doing our rollout in stages, and could change process before all systems were affected. New process was to ensure that we unchecked the dynamic registration option. All under control, and the rollout continued.


## OK, so how come half of all CLIENTS just went offline?


All went well, until one day we started getting reports that clients were having trouble accessing resources across the network. Clients couldn’t login, or apps would time out. There was no obvious pattern - it wasn’t location-based. One person would be working fine, the person next to them would be effectively offline. Rebooting sometimes worked, sometimes it didn’t.

The most recent network change was to add the backup interface to an AD server. That narrowed our investigations a bit. Eventually we realised that some clients were trying to use the wrong interface of our AD server for DNS queries. But how did they learn that? We’d un-checked the dynamic registration option. But when we looked at our DNS zones, we could see multiple entries for our AD servers. Gah!

Turns out there’s an additional step required if you’ve got a secondary NIC on your AD server. You need to configure the DNS service to not listen on the backup interface. For each zone, you also need to remove the unwanted IP from the “Name Servers” tab. See [KB2023004](https://support.microsoft.com/en-us/kb/2023004) for more info.

That one was lots of fun, especially when EVERYTHING seemed to be melting!
