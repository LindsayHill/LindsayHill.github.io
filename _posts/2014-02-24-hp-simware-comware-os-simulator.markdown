---
author: lindsay
date: 2014-02-24 18:00:46+00:00
layout: post
slug: hp-simware-comware-os-simulator
title: HP Simware - Comware OS Simulator
categories:
- Routing &amp; Switching
tags:
- Comware
- hp
- study
---

HP recently released "Simware", a Comware network simulator that lets you create test networks of emulated Comware switches and routers. This can be used to create "virtual" networks, ideal for learning, feature validation, and change testing. It's broadly similar to Dynamips. It's not perfect, and it's been a long time coming, but it's a start.

## Features

* Simulate Comware routers, stackable switches, and modular switches with switching and routing modules.
* Runs on Windows & Ubuntu, 32- and 64-bit.
* Simulated network can span multiple host systems, and/or can connect to 'real' network devices. Theoretically you could integrate it with GNS3, IOS-XRv, VSR, etc.
* Supports most features - e.g. TRILL, OSPF, BGP, IS-IS, MPLS, LACP, VRRP, MSTP
* Supports IRF and multi-tenant device context.

{% include note.html content="Note: This does not emulate specific hardware the way GNS3 does. It is not running 'real' firmware images - they are customised for this tool." %}

## Obtaining Simware

The simulator is available from the [HP Partner Dropbox](https://h30620.www3.hp.com/login-channel ). You will need to register for this, and yet again, it does not use HP Passport (Passport is supposed to be the one login I can use across all HP sites: I currently have 5 different HP-related logins). For some reason, you also need to enter a registration code, which is on [this page](http://h40060.www4.hp.com/email/eg/apr-13/pages/tools.html#5). It appears that it may be technically restricted to HP Employees and Partners only at this stage. I believe they intend to release a customer version in future.

Once you have access to the Dropbox, you can pick your version (Windows or Ubuntu). Download size varies between around 200MB & 300MB. Unpack the files, read the instructions, and run the installer.

## Running Simware

There's three steps to running Simware:

1. Create a configuration file that details the devices & boards you're going to run, the console port, and the interconnections between devices (both physical and virtual devices, and possibly spanning hosts)
2. Compile the configuration - this will create flash & nvram files for each device, and a startup script.
3. Launch it - start each individual switch/router.

### Configuration:

I'm just going to run a simple configuration - two 48x1GbE switches, connected together using the first port on each switch. Here's my config:

```ini
device_id = 1
device_model = SIM2200
board = SIM2201 : aux_port 3000

device_id = 2
device_model = SIM2200
board = SIM2201 : aux_port 3001

device 1 : interface 1 <---> device 2 : interface 1
```

I've saved that as `test.cfg` in `C:\Program Files (x86)\Simware`.

### Compilation:

Run `cfgtool.exe`:

```text
Please specify a config file.
test.cfg
Build successfully.
Press any key to continue . . .
```

Note that you may need to use "Run as Administrator", depending on your Windows security settings.

### Launch it

Go into `run_env\test.cfg`. You'll see a folder for each device, and a startup script:

```posh
PS C:\Program Files (x86)\Simware> cd .\run_env\test.cfg
PS C:\Program Files (x86)\Simware\run_env\test.cfg> dir


    Directory: C:\Program Files (x86)\Simware\run_env\test.cfg


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----       15/02/2014  6:40 p.m.            device1
d----       15/02/2014  6:40 p.m.            device2
-a---       15/02/2014  6:40 p.m.        899 device1.vbs
-a---       15/02/2014  6:40 p.m.        899 device2.vbs

```

The .vbs scripts are basically just simple scripts to launch QEMU with all the right options. Run each of those individually to launch each simulated device. You'll get a pop-up with a console, and shortly you'll be able to use PuTTy to Telnet to the right port on your loopback. That connects you to the Auxiliary port of each device.

## Initial Impressions

It's slow. It's very, very slow. It takes a long time to boot, and commands can take quite a while to respond. This is due to the use of QEMU - it is emulating hardware, which is a very different proposition to running natively compiled code, like Cisco IOU. The "switches" in my testing were automatically allocated a maximum of 768MB of RAM, and each device maxed out a CPU core.

I would have liked to have done more testing, but the interface speed was driving me nuts. This may be because I was running it on a Windows 2008 VM on an ESXi host. It might be better if I was running it on Windows or Ubuntu running natively. I don't have any Windows or Linux systems that run directly on physical hardware, so I will have to try compiling QEMU for Mac OS X.

## Future Enhancements?

Here's some ideas on how this tool could be improved:

* **Speed:** The speed has to improve. It might simply be that HP needs to provide some guidance, saying "Allow _X_ and _Y_ resources per switch, be aware that features _A_ _B_ and _C_ will severely impact performance"
* **GUI:** Create a simple front-end GUI for generating configuration files, compiling and launching them. Simple point & click / drag & drop for creating topologies, etc. This should be quite easy, but I understand why it has not been a priority - anyone who has half a clue can fairly quickly create config files. Related to this, the overall packaging needs to be cleaned up - currently it feels more like a developer's quick hack than a production-ready tool.
* **Configuration Management:** Building up a complete configuration for each new network is a pain. Maybe provide a way of saying "Here's my addressing scheme. Run OSPF on these 4 four devices, MSTP on these 2 switches, and create an IRF cluster with those 3 switches." This would be fantastic for testing, as it would let you focus on the features you want to test, not waste your time building up base configurations.
* **ProCurve:** Add emulation for ProCurve switches.
* **"Meta-Sim":** Blue-sky thinking here - What about a tool that could pull together the various network simulators and virtual routers/switches from HP/Cisco/Juniper/Brocade/etc, and give you a co-ordinated way of spinning up large multi-vendor networks? Problem is that no vendor is going to do it, it will have to come from the community. GNS3 would be the obvious source, but I'm somewhat bearish about their future.

## Would I Use It?

I probably won't use this simulator much, at least until I can figure out a way to make it run at a usable speed. If I need to do any testing with Comware, I will run the [VSR1000]({% post_url 2013-10-14-hp-vsr1000-getting-started %}) as a VM. I will only use this simulator if I need to test out switching features, or IRF. Still, I'm pleased that HP has finally started delivering these much-needed resources. I think that [Cisco CML](http://blog.ipspace.net/2013/10/cisco-modeling-lab-virl-behind-scenes.html) will significantly raise the bar for engineer's expectations of a network simulation tool, and HP can't afford to be left behind. Here's hoping they can improve it - or better yet, help the community to improve it.
