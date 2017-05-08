---
author: lindsay
date: 2014-02-17 19:45:11+00:00
layout: post
slug: hp-vsr-portal-redirection
title: HP VSR Portal Redirection
categories:
- Routing &amp; Switching
tags:
- hp
- imc
- VSR
---

When implementing HP IMC UAM, you may need to redirect users to the IMC webserver, for device registration & configuration, and obtaining user 802.1x certificates. One method of doing this is to put users into an initial "Registration" VLAN, and configure an HP Comware device to redirect all web traffic. Here's how to do it using HP's VSR Virtual Router.


## The Scenario


Here's the network we're working with:

[![VSR Redirection](/assets/2014/02/VSR-Redirection.png)](/assets/2014/02/VSR-Redirection.png)

We've set up an HP VSR virtual router running on ESXi. This will be the default gateway for our test PC in the 192.168.200.0/24 VLAN. We want to capture any outbound HTTP requests to Google etc, and redirect them to our IMC server at 172.16.2.75.

The DHCP server has been configured with a scope for the 192.168.200.0/24 VLAN, and DHCP helper functionality has been configured on the VSR. The DHCP server will tell clients to use 192.168.200.1 as their default gateway, and 172.16.2.40 as their DNS server. I have added static routes on the DHCP & IMC servers pointing to 192.168.200.0/24 via 172.16.2.1. Of course I wouldn't use this setup in production, but it's just a quick test network.



## VSR Configuration



I won't cover the generic router configuration here - I'll just focus on the redirect functionality. There's two parts to it: Define the traffic that should bypass redirection (in this case DNS), and then define the redirect destination. Then we apply the configuration to the interface facing VLAN 200. Simple as that!

Here's our 'portal' configuration:


```text
 portal free-rule 0 source ip any destination ip 172.16.2.40 255.255.255.255
#
portal web-server IMC
 url http://172.16.2.75/imc
```


And here we apply those rules to the interface:


```text
dis cur int g2/0
#
interface GigabitEthernet2/0
 ip address 192.168.200.1 255.255.255.0
 dhcp select relay
 dhcp relay server-address 172.16.2.40
 portal enable method direct
 portal apply web-server IMC
#
```




## Testing the Config



Let's jump onto our test PC. First we'll check that we've been given a working IP, and that DNS is still working:


```bash
[lindsay@testpc ~]$ ip addr list eth0
2:  eth0: <broadcast,multicast,up,lower_up> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 00:50:56:bb:cc:dd brd ff:ff:ff:ff:ff:ff
    inet 192.168.200.100/24 brd 192.168.200.255 scope global eth0
    inet6 fe80::250:56ff:feb7:3239/64 scope link
       valid_lft forever preferred_lft forever
[lindsay@testpc ~]$ host packetpushers.net
packetpushers.net has address 162.159.248.87
packetpushers.net has address 162.159.247.87
packetpushers.net has IPv6 address 2400:cb00:2048:1::a29f:f857
packetpushers.net has IPv6 address 2400:cb00:2048:1::a29f:f757
```


That part all looks pretty good. Now watch what happens we open Firefox, and go to [duckduckgo.com](http://www.duckduckgo.com/):

[![Firefox redirected](/assets/2014/02/firefox-redirect.jpg)](/assets/2014/02/firefox-redirect.jpg)

{:.image-caption}
Firefox redirected

We've been redirected to http://172.16.2.75/imc. Perfect. I don't have a full HP BYOD setup here, just a regular IMC server. If you were doing this with HP UAM, you'd be redirecting to something like http://172.16.2.75/byod/

Note that this configuration as it stands is not fool-proof - HTTPS traffic will completely bypass it. Explore the other "portal" configuration options, and you'll find ways of blocking this if needed.

The nice thing about this solution is that you can use a free instance of HP's VSR - it's bandwidth limited, but that really doesn't matter when you're just doing portal redirect. Normally you'll authenticate your users, and then push them across to another VLAN that doesn't use the VSR.
