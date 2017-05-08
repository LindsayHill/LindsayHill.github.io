---
author: lindsay
date: 2015-07-21 03:48:45+00:00
layout: post
slug: add-brocade-mlx-vdx-support-to-hp-imc
title: Add Brocade MLX & VDX Support to HP IMC
categories:
- HP IMC
tags:
- Brocade
- hp
- imc
---

HP IMC 7.1 E0303P13 does not support configuration backups for Brocade MLX & VDX devices. But they do have an extensible model, so it's easy to add support. Here's how to do it, and how to fix the Brocade ICX support.

Here's the steps to add support for MLX & VDX devices to HP IMC:



  1. Download the [current set](https://github.com/NetOpsCommunity/imc-scripts/archive/master.zip) of adapters from GitHub.

  2. Unpack the zip file, and copy the adapters into place.

  3. Add Device Series & Device Model definitions.

  4. Restart IMC, re-synchronise, and check file transfer modes.


Going into a bit more detail:

{% include note.html content="NB: Yes, I do work for Brocade. That doesn't mean that these adapters are fully supported by Brocade. I'll help out however I can, but can't promise anything." %}



## NetOps Custom Adapters


This [GitHub repository](https://github.com/NetOpsCommunity/imc-scripts/) maintains a set of 3rd-party developed adapters for HP IMC. You can download individual files, create a local copy of the repo using Git, or just download a zip file containing all current scripts from [here](https://github.com/NetOpsCommunity/imc-scripts/archive/master.zip).

On the IMC server, adapters are stored at `(IMC)/server/conf/adapters/ICC`. You'll see directories for all supported vendors there:


```sh
[root@imc ~]# cd /opt/iMC/server/conf/adapters/ICC
[root@imc ICC]# ls
3Com    Alcatel-Lucent  Aruba Networks  Avocent  Cabletron  Dell  Enterasys         F5       Fortigate  H3C              Hillstone  IBM               Lantronix  Motorola  Nortel networks  PaloAlto  Tasman
Adtran  Allied Telesis  Avaya           Brocade  Cisco      Digi  Extreme networks  Force10  Foundry    Hewlett Packard  Huawei     Juniper networks  Marconi    MP        Packeteer        Ruijie    ZTE
[root@imc ICC]#
```


Under each vendor directory is an `adapter-index.xml` file containing mappings to specific adapters, and a directory for each specific adapter. So we can see here that Cisco has a number of different adapters for different Cisco devices:


```sh
[root@imc ICC]# ls Cisco
adapter-index.xml  CiscoAS54xx  CiscoC2980G     CiscoESR     CiscoIOS7K       CiscoIOSGenericNoLog   CiscoIOSL3Switch             CiscoIOSUndiscovered  CiscoNX7K      CiscoSNMP
CiscoAC            CiscoASA     CiscoCatNative  CiscoIOS12K  CiscoIOSGeneric  CiscoIOSGenericSwitch  CiscoIOSSwitchRoutingModule  CiscoIOS_XR           CiscoNXOSSNMP
[root@imc ICC]#
```


From either your local copy of the repo, or the downloaded zip file, find the `adapters/ICC/Brocade` directory. Copy `adapter-index.xml`, `Vyatta`, `BrocadeMLX` and `BrocadeNOS` to `(IMC)/server/conf/adapters/ICC/Brocade` on your IMC server.


## Device Definitions


If you discover a Brocade VDX system with HP IMC, it will be monitored via SNMP. But it shows up as "Brocade Generic." Similarly, an MLX will show up as "Foundry Generic." This is mostly just a cosmetic annoyance (but make sure the displayed vendor name matches the [adapter directory]({% post_url 2015-05-16-hp-imc-adapter-directory-naming %})). Here's how to fix that:



  1. Open the IMC web interface. Go to System -> Resource Management -> Device Series

  2. Click "Add" - add new series name "Brocade MLX." Set the vendor to Brocade, and Description to "Brocade NetIron MLX."

  3. Repeat the above for "Brocade VDX"

  4. Go to Device Model. Add new sysOID -> Device Model mappings. Make sure you pick the right vendor & series.


If you've already discovered devices in IMC, you can select one to copy the sysOID from. Here's some MLX & VDX sysObjectIDs:


  * MLX:

    * MLX16Rt - 1.3.6.1.4.1.1991.1.3.44.1.2

    * MLX8Rt - 1.3.6.1.4.1.1991.1.3.44.2.2

    * MLX4Rt - 1.3.6.1.4.1.1991.1.3.44.3.2

    * MLX-32Rt - 1.3.6.1.4.1.1991.1.3.44.4.2

    * MLXe-16Rt - 1.3.6.1.4.1.1991.1.3.55.1.2

    * MLXe-8Rt - 1.3.6.1.4.1.1991.1.3.55.2.2

    * MLXe-4Rt - 1.3.6.1.4.1.1991.1.3.55.3.2

    * MLXe-32Rt - 1.3.6.1.4.1.1991.1.3.55.4.2
    
  * VDX:

    * VDX6740 - 1.3.6.1.4.1.1588.3.3.1.131

    * VDX6747 - 1.3.6.1.4.1.1588.2.2.1.1.1.112

    * VDX6740T - 1.3.6.1.4.1.1588.3.3.1.137

    * VDX2740 - 1.3.6.1.4.1.1588.3.3.1.138

    * VDX6740T1G - 1.3.6.1.4.1.1588.3.3.1.151

    * VDX7770P36 - 1.3.6.1.4.1.1588.3.3.1.153

    * VDX7770P72 -  1.3.6.1.4.1.1588.3.3.1.154

    * VDX8770S4 - 1.3.6.1.4.1.1588.3.3.1.1000

    * VDX8770S8 - 1.3.6.1.4.1.1588.3.3.1.1001

    * VDX8770S16 - 1.3.6.1.4.1.1588.3.3.1.1002



## Restart & Resynchronise


You'll need to restart IMC so it picks up the changes to device adapter definitions. It will also need to resynchronise the devices to pick up the changes. This will happen automatically within a few hours, or you can force it.

The MLX adapter supports SCP. I strongly recommend setting the file transfer mode to SCP. This is better than using the CLI adapters.

Now you should be able to back up those devices.


## ICX Adapter Updates


HP IMC ships with adapters for the Brocade/Foundry ICX FastIron systems. This adapter does not support SCP, and there are issues with the CLI fall-back adapter. If you want to apply these changes, copy the `adapters/ICC/Foundry/FoundryNetIron` directory from the GitHub repo to `(IMC)/server/conf/adapters/ICC/Foundry` and restart IMC.

Now you can use SCP for those devices too.

These adapters have had limited testing. Device backup works, and Configuration deployment should work. Firmware deployment has not been tested. If you have any problems, either [get in touch with me](mailto:lindsay@lkhill.com), or submit a Pull Request.
