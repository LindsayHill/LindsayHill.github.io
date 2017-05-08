---
author: lindsay
date: 2015-04-27 03:18:53+00:00
layout: post
slug: netbeez-review
title: NetBeez Review
categories:
- NMS
tags:
- NFD
---

[NetBeez](http://netbeez.net/) presented at [Network Field Day 9](http://techfieldday.com/event/nfd9/), where they showed us their solution for distributed [network performance monitoring](http://datanetworkingtalk.com/what-it-is-and-what-it-beez-like/). They gave the delegates a NetBeez agent to take home and test. I've run it for the last two months, and I've been happy with how it has performed.



## Physical Install



The unit was supplied with a US power plug. I was contemplating using an adapter, but I didn't have any spare power points near where I wanted to install it. Hmmm. Then I realised that the power connection is just a USB port anyway. The Ethernet cable needed to go into my [SRX-110](http://www.juniper.net/us/en/products-services/security/srx-series/srx110/), so I wonder if...

[![NetBeez and SRX](/assets/2015/04/NetBeez-and-SRX.jpg)](/assets/2015/04/NetBeez-and-SRX.jpg)

Yup! Powers from the USB port on the SRX. Perfect.



## Monitoring Setup



The device powered on, and it soon showed up on the NetBeez web dashboard. This is where you can configure your agents, define what tests you want them to run, and see the results.

I added a few simple monitors:


  * HTTP monitoring to this [website]({{ site.baseurl }})

  * HTTP monitoring to [www.netopscommunity.net](http://www.netopscommunity.net/)

  * PING monitoring to [www.netopscommunity.net](http://www.netopscommunity.net/)


All very straightforward to add the monitors, and pick which agents to run them from. Running the same tests from multiple agents gives you a distributed status view. This can help you figure out if it's a network, server or user problem. I've got tests running from my agent, and from [@mrtugs'](http://www.movingpackets.net/) agent.



## Results



I now have a few months of data, and I've been happy with what it's giving me. Here's some stats for Saturday evening:

[![LKHill Stats](/assets/2015/04/LKHill-Stats.png)](/assets/2015/04/LKHill-Stats.png)

Note those breaks in the data? That was when I was reconfiguring my SRX. Take a look at the same time window, from a different agent:

[![LKHill from Mr Tugs](/assets/2015/04/LKHill-from-Mr-Tugs.png)](/assets/2015/04/LKHill-from-Mr-Tugs.png)

No breaks, and you can also see that response times seem to be a little lower there. Here's the summary report for the last week for that test:

[![lkhill http report](/assets/2015/04/lkhill-http-report.png)](/assets/2015/04/lkhill-http-report.png)

The left-hand green bar is the average response time from John's agent, and the right-hand bar is response time from my house. New Zealand might be a beautiful country, but it's a long way away from anywhere.

You can also see the red bars for failures, and orange bars for alerts. Note that not all failures trigger alerts. This is because the default setting is for HTTP failures to only alert if there are 5 consecutive failures. This is of course configurable. You probably don't want alerts for one or two failures from an agent, as you'll get a bit of noise. But if it's been down for 5 times in a row, then someone should probably look at that.

All very easy to configure, and it does what it says it will. No ongoing management hassles either. NetBeez have rolled out updates to the dashboard & the agents during the last few weeks, and I haven't had to touch them.

{% include note.html content="NetBeez were a sponsor of NFD9. This meant that they indirectly covered my costs to attend." %}

