---
author: lindsay
date: 2014-02-09 21:00:33+00:00
layout: post
slug: cant-start-hp-imc-on-linux
title: Can't Start HP IMC on Linux?
categories:
- HP IMC
tags:
- hp
- imc
---

I was recently asked about how to start IMC on a Linux server. After the initial installation, the system had been restarted, and now IMC wasn't running. How to start it again?

All of my previous installations had been on Windows, where I knew exactly what to do. I knew the process would be almost identical on Linux, but I wasn't sure of exactly which commands were required. So I spun up a VM, installed MySQL, and installed IMC 7.0.

At the end of the installation, you'll see the "Deployment Monitoring Agent", looking something like this:

[![Deployment Monitoring Agent IMC Stopped](/assets/2014/02/Deployment-Monitoring-Agent-IMC-Stopped.png)](/assets/2014/02/Deployment-Monitoring-Agent-IMC-Stopped.png)

If you click "Start IMC", it will start, and all will be good...until you reboot. If you did NOT check "Automatically start the services when the OS starts", IMC will not be running post-reboot. How to start it again? You need to run the Deployment Monitoring Agent. You will need X-Windows for this. You can login to the console of the server - if it's not running a graphical console, run `startx` after logging in. Or you can tunnel it back to a local X-Windows server - e.g. run XQuartz on your Mac, and then ssh to the server with `ssh -X user@IMC`.

Once you've got that working, run `/opt/iMC/deploy/dma.sh` - that will pop up the Deployment Monitoring Agent, and you can start IMC. You probably also want to set it to auto-start in future:

[![IMC Deployment Monitoring Agent Started](/assets/2014/02/IMC-Deployment-Monitoring-Agent-Started.png)](/assets/2014/02/IMC-Deployment-Monitoring-Agent-Started.png)

Note: You may have noticed the `imcdmsd` service added to your system:

```bash
[root@IMC ~]# chkconfig --list imcdmsd
imcdmsd         0:off   1:off   2:off   3:on    4:off   5:on    6:off
[root@IMC ~]#
```

This service must be running before the Deployment Monitoring Agent will work. It should be started by default. If not, start it with `service imcdmsd start`. If you have set IMC to auto-start, starting this service will also start IMC.
