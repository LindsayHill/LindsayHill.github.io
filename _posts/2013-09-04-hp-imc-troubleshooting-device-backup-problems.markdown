---
author: lindsay
date: 2013-09-04 19:00:19+00:00
layout: post
slug: hp-imc-troubleshooting-device-backup-problems
title: 'HP IMC: Troubleshooting Device Backup Problems'
categories:
- HP IMC
tags:
- backups
- imc
- SSH
- Telnet
---

HP IMC is great at backing up your switch configurations, but it can be very frustrating when it doesn't work. Here's a list of steps I go through when debugging device backups:

1. What specific vendor and model device? Are these supported by IMC, or do we need a custom adapter? I'll write a post in future that walks through developing a custom adapter. Otherwise, go on with the next steps.
2. Note the Login Type (Telnet or SSH, set on Device Details page).
3. Note the file transfer mode - IMC has a default mode, and a per-device mode that overrides the default. Options for both are TFTP, FTP, SFTP and SCP. If your device can do SFTP or SCP, it probably needs extra configuration - e.g. "ip scp server enable" on Cisco devices. IMC File Transfer mode is set at Service -> Configuration Center -> Options. Note the file transfer direction - if you use TFTP or FTP, the device pushes the config to the IMC server. If you use SCP or SFTP, IMC pulls configuration from the device.
4. Login to the IMC server via RDP or SSH. Go to /server/bin. Run "telnet " or "plink " and try and connect to your device. This step is testing that ACLs and routes between IMC and the device are correctly configured.
5. Check that IMC has the right credentials for your device. Go to System -> System Configuration -> System Settings, and enable Plain Text display of passwords.
6. Go to the Device Details page, check the Login Type is correct, and look at the SNMP/Telnet credentials to make sure they are correct. Note that if you need an enable password on a Cisco device, this is called "Super/Manager Password".
7. Is your device configured for SNMP read-write? If it's not, then delete the SNMP Read-Write Community String on the Device Details page. For most devices, this is not required, but if you've set a Read-Write string in IMC, but not on the device, it will just slow it down while it first tries SNMP set commands, and waits for it to timeout.
8. If you're using Telnet, run Wireshark on the IMC server, and capture the traffic between IMC and the device. Trigger a backup, and look to see that the session flow is what you expect.
9. Other tips: If at all possible, move to SCP or SFTP. There's fewer moving parts to this, and it is generally more reliable, as well as more secure. If you must use FTP, then make sure that the FTP server is running on the IMC server, that the home directory is set to /server/tmp, and actually try to upload a file. Don't get caught out by incorrect folder permissions.

If you've gone through all of those, and it still doesn't work, you should look at `<IMC>/server/conf/log/imccfgbakdm*`, to see what's going on.

Going through those steps will cover most backup failures. The next step is to start looking into the adapters themselves, and understanding their traffic flow. That's for another post, along with developing custom adapters.

Remember, if you're having troubles, try posting something at [www.netopscommunity.net](http://www.netopscommunity.net) - we can probably help.
