---
author: lindsay
date: 2016-10-28 01:16:37+00:00
layout: post
slug: vrf-aware-snmp-on-brocade-vdx
title: VRF-Aware SNMP on Brocade VDX
categories:
- NMS
tags:
- Brocade
- snmp
---

SNMP was not designed with VRFs in mind. Querying the routing table via SNMP did not take into account the idea of having multiple routing tables. But clearly it’s something people want to do, so some clever engineers figured out how to shoe-horn VRF contexts in. This week a customer asked me how to query the routing table for the non-default VRF on Brocade VDX switches. Here’s how to do it:



## VRF Configuration



{% include warning.html content="I’m using a Brocade 6940 running NOS 7.0.1 here. Note that SNMP configuration changed around NOS 6.x, so if you’re running something older this may work differently." %}


For this lab I have Loopback 1 in the default VRF, with an IP of 50.50.50.50/32. I’ve created another VRF called “internet”, and put Loopback 2 in that VRF, with IP 60.60.60.60/32. Now I have two different routing tables:


```sh
VDX6940-204063# sh run rb 1 int loop 1
rbridge-id 1
interface Loopback 1
no shutdown
ip address 50.50.50.50/32
!
!
VDX6940-204063# sh ip route
Total number of IP routes: 1
Type Codes - B:BGP D:Connected O:OSPF S:Static U:Unnumbered +:Leaked route; Cost - Dist/Metric
BGP Codes - i:iBGP e:eBGP
OSPF Codes - i:Inter Area 1:External Type 1 2:External Type 2 s:Sham Link
Destination Gateway Port Cost Type Uptime
50.50.50.50/32 DIRECT Lo 1 0/0 D 1d1h
VDX6940-204063# show running-config rbridge-id 1 vrf internet
rbridge-id 1
vrf internet
rd 65500:100
address-family ipv4 unicast
!
!
!
VDX6940-204063# show running-config rb 1 int loop 2
rbridge-id 1
interface Loopback 2
no shutdown
vrf forwarding internet
ip address 60.60.60.60/32
!
!
VDX6940-204063# sh ip route vrf internet
Total number of IP routes: 1
Type Codes - B:BGP D:Connected O:OSPF S:Static U:Unnumbered +:Leaked route; Cost - Dist/Metric
BGP  Codes - i:iBGP e:eBGP
OSPF Codes - i:Inter Area 1:External Type 1 2:External Type 2 s:Sham Link
Destination        Gateway         Port           Cost          Type Uptime
60.60.60.60/32     DIRECT          Lo 2           0/0           D    13m16s
```




## Default SNMP Configuration



Basic SNMP configuration looks like this:


```sh
snmp-server community public groupname user
snmp-server view All 1 included
snmp-server group user v1 read All write All notify All
snmp-server group user v2c read All write All notify All

```


If we poll the inetCidrRouteTable OID (1.3.6.1.2.1.4.24.7), we can see results for the default VRF:


```sh
~ lhill$ snmpwalk -v 2c -c public 10.254.4.149 1.3.6.1.2.1.4.24.7
IP-MIB::ip.24.7.1.7.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 1476395009
IP-MIB::ip.24.7.1.7.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.7.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.8.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 3
IP-MIB::ip.24.7.1.8.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 4
IP-MIB::ip.24.7.1.8.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 4
IP-MIB::ip.24.7.1.9.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 2
IP-MIB::ip.24.7.1.9.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 1
IP-MIB::ip.24.7.1.9.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 1
IP-MIB::ip.24.7.1.10.1.4.50.50.50.50.32.2.0.0.0.0 = Gauge32: 93453
IP-MIB::ip.24.7.1.10.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = Gauge32: 8020667
IP-MIB::ip.24.7.1.10.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = Gauge32: 8020667
IP-MIB::ip.24.7.1.11.1.4.50.50.50.50.32.2.0.0.0.0 = Gauge32: 0
IP-MIB::ip.24.7.1.11.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = Gauge32: 0
IP-MIB::ip.24.7.1.11.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = Gauge32: 0
IP-MIB::ip.24.7.1.12.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.12.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.12.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.13.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.13.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.13.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.14.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.14.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.14.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.15.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.15.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.15.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.16.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.16.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.16.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.17.1.4.50.50.50.50.32.2.0.0.0.0 = INTEGER: 1
IP-MIB::ip.24.7.1.17.2.16.254.128.0.0.0.0.0.0.0.0.0.0.0.0.0.0.10.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 1
IP-MIB::ip.24.7.1.17.2.16.255.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.8.2.0.0.2.16.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0 = INTEGER: 1
~ lhill$
```




## VRF-Aware SNMP



OK, so how do we poll the ‘internet’ VRF routing table?

We need to configure [multiple SNMP server context to VRF mapping](http://www.brocade.com/content/html/en/administration-guide/nos-701-adminguide/GUID-A557CDAE-F5CD-4FC1-B33C-7385D1243226.html). Here’s a minimal configuration (added to our earlier config):


```sh
snmp-server community internet groupname user
snmp-server context internetcontext
vrf-name internet
!
snmp-server mib community-map internet context internetcontext
```


Now we can poll our switch using the community string `internet`, and we’ll get results for that VRF:


```sh
lhill$ snmpwalk –v2c -c internet 10.254.4.149 1.3.6.1.2.1.4.24.7
IP-MIB::ip.24.7.1.7.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 1476395010
IP-MIB::ip.24.7.1.8.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 3
IP-MIB::ip.24.7.1.9.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 2
IP-MIB::ip.24.7.1.10.1.4.60.60.60.60.32.2.0.0.0.0 = Gauge32: 422
IP-MIB::ip.24.7.1.11.1.4.60.60.60.60.32.2.0.0.0.0 = Gauge32: 0
IP-MIB::ip.24.7.1.12.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.13.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 0
IP-MIB::ip.24.7.1.14.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.15.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.16.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: -1
IP-MIB::ip.24.7.1.17.1.4.60.60.60.60.32.2.0.0.0.0 = INTEGER: 1
lhill$
```


Easy as that.
