---
author: lindsay
date: 2014-11-23 05:57:35+00:00
layout: post
slug: juniper-srx-110h-eol
title: Juniper SRX-110H EoL
categories:
- Security
tags:
- fibre
- SRX
- UFB
---

Somehow I missed this when it was announced, but the Juniper SRX-110H-VA is End of Life, and is no longer supported for new software releases.

End of Life announcement is [here](http://kb.juniper.net/InfoCenter/index?page=content&id=TSB16275), with extra detail in this [PDF](http://kb.juniper.net/resources/sites/CUSTOMERSERVICE/content/live/TECHNICAL_BULLETINS/16000/TSB16275/en_US/TSB16275.pdf). Announcement was Dec 10 2013, with "Last software engineering support" date Dec 20 2013.

This is now starting to take effect, with 12.1X47 [not supported on this platform](http://www.juniper.net/techpubs/en_US/junos12.1x47/information-products/topic-collections/release-notes/12.1x47/index.html):

> Note: Upgrading to Junos OS Release 12.1X47-D10 or later is not supported on the J Series devices or on the low-memory versions of the SRX100 and SRX200 lines. If you attempt to upgrade one of these devices to Junos OS 12.1X47-D10, installation will be aborted with the following error message:
> "ERROR: Unsupported platform <platform-name >for 12.1X47 and higher."

The replacement hardware is the SRX-110H2-VA, which has 2GB of RAM instead of 1GB. Otherwise it's exactly the same, which seems a missed opportunity to at least update to local 1Gb switching.

Michael Dale has a little more info [here](http://michaeldale.com.au/archive/2014/08/23/junos-121x47-first-gen-srx-devices-are-no-longer-supported/), along with tips for tricking a 240H into [installing 12.1X47](http://michaeldale.com.au/archive/2014/08/23/running-junos-121x47-on-first-gen-srx240h/).

> So I decided to see if I could work around this and trick JunOS into installing on my 240H, I was successful :D
> 
> I wouldn't recommend ever using this in production, but I am sure it will work fine for the lab. The only difference between the 240H and the 240H2 is that the H2 has 2GB flash and 2GB ram, CPU is the same.

I was never fully happy with my SRX, with the DHCPv6 support being very poor, and still unstable. I'll probably sell the SRX early in the new year. Might replace it with a TP-Link 7980, or similar. All I really need is ADSL2+, and decent IPv6 support. Doesn't even matter about UFB support any more. They ran the ducts outside my house [earlier this year]({% post_url 2014-03-21-fibre-future %}), but now the [map](http://www.chorus.co.nz/maps) says that service won't be available until "between Jul 2018 and Jun 2019." A block away they already have VDSL, and will have 200Mb UFB by April 2015. Damn.
