---
author: lindsay
date: 2020-01-12
layout: post
slug: snmp-routing-instances
title: Junos SNMP via Routing Instance
categories:
- NMS
tags:
- Juniper
- SNMP
---

Juniper [routing instances](https://www.juniper.net/documentation/en_US/junos/topics/concept/routing-instances-overview.html) are very useful when you need separate routing tables on the one device, for example to separate customers. Junos lets you configure SNMP polling of routing instances, so customers can poll "their" interfaces using `'instance_name'@'community'`. All very useful. But it wasn't obvious to me how to poll the **default** table via an interface in a routing instance. The trick is to just use `@'community'`. Here's an example.

## Network Overview

To demo this I have a simple network. I'm using a Virtual QFX plus Vagrant setup, based on the Vagrantfiles in [this repo](https://github.com/Juniper/vqfx10k-vagrant). I'm running one vqfx10k, connected to one server. The key here is that the server has two connections to the vqfx. One interface is in the default instance, one is in a "Customer" routing instance:

[![network_overview](/assets/2020/01/network_overview.png)](/assets/2020/01/network_overview.png)

Here's the `routing-instance` config:

```json
vagrant@vqfx> show configuration routing-instances
Customer {
    instance-type virtual-router;
    interface xe-0/0/1.0;
}

{master:0}
vagrant@vqfx>
```

And here's my SNMP configuration:

```json
vagrant@vqfx> show configuration snmp
filter-interfaces {
    interfaces {
        lsi;
        dsc;
        tap;
        gre;
        ipip;
        pim*;
        esi;
        cp*;
        irb*;
        pip*;
        jsrv*;
        mtun*;
        gr*;
        vtep;
    }
    all-internal-interfaces;
}
community public {
    authorization read-only;
    routing-instance Customer;
}
routing-instance-access;

{master:0}
vagrant@vqfx>
```

Key lines there are `routing-instance-access` - which allows SNMP polling via routing instances - and `routing-instance Customer` under `community public`. The first allows polling via non-default instance, and the second says we can use `public` for polling via the "Customer" instance. You could have unique communities per instance, or some other more complex set of SNMP ACLs.

## SNMP Polling - Default Instance

Let's start with polling the default instance, via `xe-0/0/0`:

```shell
vagrant@server:~$ snmpwalk -v 2c -c public 10.1.2.1 ifDescr
IF-MIB::ifDescr.6 = STRING: lo0
IF-MIB::ifDescr.16 = STRING: lo0.0
IF-MIB::ifDescr.17 = STRING: em0
IF-MIB::ifDescr.18 = STRING: em0.0
IF-MIB::ifDescr.23 = STRING: em1
IF-MIB::ifDescr.24 = STRING: em1.0
IF-MIB::ifDescr.35 = STRING: vme
IF-MIB::ifDescr.152 = STRING: em3
IF-MIB::ifDescr.156 = STRING: em5
IF-MIB::ifDescr.158 = STRING: em6
IF-MIB::ifDescr.160 = STRING: em7
IF-MIB::ifDescr.515 = STRING: xe-0/0/1
IF-MIB::ifDescr.516 = STRING: xe-0/0/0
IF-MIB::ifDescr.517 = STRING: xe-0/0/0.0
IF-MIB::ifDescr.518 = STRING: xe-0/0/2
IF-MIB::ifDescr.519 = STRING: xe-0/0/3
IF-MIB::ifDescr.520 = STRING: xe-0/0/4
IF-MIB::ifDescr.521 = STRING: xe-0/0/5
IF-MIB::ifDescr.523 = STRING: xe-0/0/6
IF-MIB::ifDescr.524 = STRING: xe-0/0/7
IF-MIB::ifDescr.525 = STRING: xe-0/0/2.16386
IF-MIB::ifDescr.526 = STRING: xe-0/0/8
IF-MIB::ifDescr.527 = STRING: xe-0/0/9
IF-MIB::ifDescr.528 = STRING: xe-0/0/3.16386
IF-MIB::ifDescr.529 = STRING: xe-0/0/10
IF-MIB::ifDescr.530 = STRING: xe-0/0/11
IF-MIB::ifDescr.531 = STRING: xe-0/0/4.16386
IF-MIB::ifDescr.532 = STRING: xe-0/0/5.16386
IF-MIB::ifDescr.533 = STRING: xe-0/0/6.16386
IF-MIB::ifDescr.534 = STRING: xe-0/0/7.16386
IF-MIB::ifDescr.535 = STRING: xe-0/0/8.16386
IF-MIB::ifDescr.536 = STRING: xe-0/0/9.16386
IF-MIB::ifDescr.537 = STRING: xe-0/0/10.16386
IF-MIB::ifDescr.538 = STRING: xe-0/0/11.16386
IF-MIB::ifDescr.539 = STRING: xe-0/0/1.0
vagrant@server:~$
```

OK, so far that's business as usual.

## SNMP Polling - Customer Instance

The Juniper documentation "[Understanding SNMP Support for Routing Instances](https://www.juniper.net/documentation/en_US/junos/topics/concept/understanding-snmp-support-for-routing-instances-junos-nm.html) shows how to specify an instance. The trick is to use `"instance"@"community"`:

```shell
vagrant@server:~$ snmpwalk -v 2c -c Customer@public 10.1.3.1 ifAlias
IF-MIB::ifAlias.515 = STRING: Customer RI
IF-MIB::ifAlias.539 = STRING:
vagrant@server:~$
```

## Polling the Default Instance via a Non-Default Interface

If I was only allowed to see my own routing instance information, the above options work fine. But what if I am polling via a non-default interface, but I want to see information for the whole device?

If I poll via the "Customer" interface, I don't get any response:

```shell
vagrant@server:~$ snmpwalk -v 2c -c public 10.1.3.1 ifAlias
Timeout: No Response from 10.1.3.1
vagrant@server:~$
```

Hmmm. What if I specify "default" as my instance name?

```shell
vagrant@server:~$ snmpwalk -v 2c -c default@public 10.1.3.1 ifAlias
IF-MIB::ifAlias.6 = STRING:
IF-MIB::ifAlias.16 = STRING:
IF-MIB::ifAlias.17 = STRING:
IF-MIB::ifAlias.18 = STRING:
IF-MIB::ifAlias.23 = STRING:
IF-MIB::ifAlias.24 = STRING:
IF-MIB::ifAlias.516 = STRING: Default RI
IF-MIB::ifAlias.517 = STRING:
vagrant@server:~$
```

That looks pretty good...except it's only showing information for the interfaces in the default table. What if I want to see **all** interfaces, the same as if I polled it via an interface in the default table?

I couldn't figure out how to do this from the docs. I think it was [Ben Dale](https://twitter.com/labelswitcher?lang=en) that suggested using `@community` - that is, just use `@` in front of the community, but don't specify an instance name:

```shell
vagrant@server:~$ snmpwalk -v 2c -c @public 10.1.3.1 ifAlias
IF-MIB::ifAlias.6 = STRING:
IF-MIB::ifAlias.16 = STRING:
IF-MIB::ifAlias.17 = STRING:
IF-MIB::ifAlias.18 = STRING:
IF-MIB::ifAlias.23 = STRING:
IF-MIB::ifAlias.24 = STRING:
IF-MIB::ifAlias.35 = STRING:
IF-MIB::ifAlias.152 = STRING:
IF-MIB::ifAlias.156 = STRING:
IF-MIB::ifAlias.158 = STRING:
IF-MIB::ifAlias.160 = STRING:
IF-MIB::ifAlias.515 = STRING: Customer RI
IF-MIB::ifAlias.516 = STRING: Default RI
IF-MIB::ifAlias.517 = STRING:
IF-MIB::ifAlias.518 = STRING:
IF-MIB::ifAlias.519 = STRING:
IF-MIB::ifAlias.520 = STRING:
IF-MIB::ifAlias.521 = STRING:
IF-MIB::ifAlias.523 = STRING:
IF-MIB::ifAlias.524 = STRING:
IF-MIB::ifAlias.525 = STRING:
IF-MIB::ifAlias.526 = STRING:
IF-MIB::ifAlias.527 = STRING:
IF-MIB::ifAlias.528 = STRING:
IF-MIB::ifAlias.529 = STRING:
IF-MIB::ifAlias.530 = STRING:
IF-MIB::ifAlias.531 = STRING:
IF-MIB::ifAlias.532 = STRING:
IF-MIB::ifAlias.533 = STRING:
IF-MIB::ifAlias.534 = STRING:
IF-MIB::ifAlias.535 = STRING:
IF-MIB::ifAlias.536 = STRING:
IF-MIB::ifAlias.537 = STRING:
IF-MIB::ifAlias.538 = STRING:
IF-MIB::ifAlias.539 = STRING:
vagrant@server:~$
```

Success! That's the output I was looking for. Easy in the end.

I couldn't see any references to this in the Juniper documentation. I've also found that this behavior changed somewhere between 15.x and 18.x. If anyone finds any docs, let me know?
