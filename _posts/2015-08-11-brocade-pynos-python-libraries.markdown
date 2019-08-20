---
author: lindsay
date: 2015-08-11 04:22:23+00:00
layout: post
slug: brocade-pynos-python-libraries
title: Brocade PyNOS Python Libraries
categories:
- Routing &amp; Switching
tags:
- Brocade
- SDN
---

PyNOS v1.1 has been published. This is a python library that simplifies automating Brocade VDX systems. It is built on top of ncclient, and uses NETCONF to communicate with the VDX systems. Using the libraries is much simpler than writing your own NETCONF calls.

## What can I do with it?

Use Python to script configuration or management tasks against VDX devices, e.g.:

* Configure interfaces & VLANs
* Find LLDP neighbors
* Find out which port a MAC is connected to
* Configure BGP
* Configure SNMP

You can also use Python as an interactive shell to run commands against multiple systems.

## Examples:

### Connect to device and check firmware version & uptime:

```python
>>> import pynos.device
>>> conn = ('172.22.90.100', '22')
>>> auth = ('admin', 'password')
>>> dev=pynos.device.Device(conn=conn, auth=auth)
>>> dev.connection
True
>>> dev.firmware_version
'6.0.1'
>>> dev.system.uptime
{'seconds': '1', 'hours': '13', 'minutes': '0', 'days': '1'}
>>>
```

### Change switchport description:

```python
>>> with pynos.device.Device(conn=conn, auth=auth) as dev:
...     dev.interface.description(
...     int_type='tengigabitethernet', name='225/0/38',
...     desc=’RTR1 Ethernet1’)
```

## Who should use it?

Any Brocade VDX customers that want to automate network configuration - e.g. to integrate with their provisioning systems.

It's helpful to have some Python experience. This is a toolbox for making some things easier - it is not a complete application in itself.

## Where Can I Get It?

To install it, run `pip install pynos`

The code is also available on GitHub: [https://github.com/brocade/pynos](https://github.com/brocade/pynos)

The documentation is here: [http://pythonhosted.org/pynos/](http://pythonhosted.org/pynos/)
