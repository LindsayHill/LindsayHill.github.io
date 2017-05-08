---
author: lindsay
date: 2013-10-10 02:00:52+00:00
layout: post
slug: why-screen-scraping-sucks
title: Why Screen Scraping Sucks
categories:
- Opinion
tags:
- Avaya
- SDN
---

There's a lot of over-blown talk these days about APIs. Everyone wants one, everyone's promising one. You might ask: "What's the difference between using an API to put a port in a specific VLAN, and just scripting the equivalent CLI commands?" Here's a couple of examples I've come across recently which show why screen-scraping (i.e. CLI scripting) sucks.


## Configuration Compliance Checks


First, Network Configuration Compliance Checks: I have recently been doing some work with HP IMC, writing Compliance policies, to ensure all network devices contain certain baseline configuration and security settings. One of the checks was checking that SNMPv3 trap settings were correct on various HP Comware systems. This is the line I was looking for:

```bash
snmp-agent target-host trap address udp-domain 10.1.1.1 params securityname SECNAME v3 privacy
```

All looks straightforward enough. I just need to write a regex pattern that matches that line. I had a couple of different trap destinations to match, but that's no big deal. But for some reason it kept saying that some of my systems were non-compliant. I couldn't figure it out, because when I would login to those devices and look at the config, it looked pretty good to me. Here's an example:

```bash
snmp-agent target-host trap address udp-domain 10.1.1.1 params securityname SECNAME v3  privacy
```

The only immediately obvious difference was that the first devices were running 1808P08, and the second ones were running 1206. But when I looked at the output more closely, I see that in the second one there's two spaces between "v3" and "privacy" (You'll probably need to scroll across within the code box to see it). Gah! Didn't seem to be mentioned in any release notes either. Simple things like an extra space can take a while to spot on the screen. I had to go through it character by character. Very, very annoying. What I really wanted was to make an API call that says "Tell me your SNMPv3 trap destinations and parameters", and then programmatically verify the output.


## Automated Backups


The second example is in writing an IMC backup adapter for Avaya ERS switches. I needed to use TCL/Expect to connect to the switch via Telnet or SSH, and either do `show run` or copy the configuration to a TFTP server. Again, straightforward enough. Sure, Avaya/Nortel is stupid, and you need to enter Ctrl+Y on initial connect, but beyond that it should be OK.

{% include warning.html content="Aside: What's the point in entering Ctrl+Y when I've just SSHed to this device? What do they think I might want to do OTHER than login? What were the developers thinking?" %}


I had some example code that would connect, then run a while loop, looking for specific output, and sending the appropriate response. For example, it would look for `Enter username:` and send the username. It would also look for various potential errors. The initial login script would know it had succeeded when it could see the Enable prompt. On an Avaya switch, the enable prompt looks like this: `AvayaSW01#`. The hostname at the start could change, so the script I was using just looked for `#` To be clear, I did not write the original script. I just fixed it.

The problem comes when connecting to the Avaya ERS. The first thing you get at initial connection is this:

```text
         ###   ###            ###   ###   ###            ###   ###
        #####   ###          ###   #####   ###          ###   #####
       ### ###   ###        ###   ### ###   ###        ###   ### ###
      ###   ###   ###      ###   ###   ###   ###      ###   ###   ###
     ###     ###   ###    ###   ###     ###   ###    ###   ###     ###
    ###       ###   ###  ###   ###       ###   ###  ###   ###       ###
   ##########  ###   ######   ##########  ###   ######   ##########  ###
  ############  ###   ####   ############  ###   ####   ############  ###
 ###             ###   ##   ###             ###  ###   ###             ###
                                                ###
                                               ###

Enter Ctrl-Y to begin.

  ***************************************************************
  *** Ethernet Routing Switch 4826GTS-PWR+                    ***
  *** Avaya                                                   ***
  *** Copyright (c) 1996-2013,  All Rights Reserved           ***
  *** SSH                                                     ***
  *** HW:04       FW:5.6.2.1   SW:v5.6.3.025                  ***
  ***************************************************************
```

What's the problem here? The script saw the initial `#` that formed "Avaya", and decided "That must be the enable prompt. Clearly my login process has already worked!" I had to tweak my script to match `[a-zA-Z0-9]+#`, and then it behaved itself.


## Common APIs: The Solution?


These are just a couple of examples of why need standardised APIs. It's not good enough to keep accepting every vendor doing their own thing, and changing it slightly as we go. Of course, APIs can and do change too. But good APIs should clearly publish version differences, in such a way that API consumers can clearly identify capabilities and variations.

We can't keep mucking around with current methods. It just consumes far too much effort, effort that could be better spent doing USEFUL things with our networks instead. Bring on common APIs!
