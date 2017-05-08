---
author: lindsay
date: 2014-02-09 01:37:30+00:00
layout: post
slug: hp-restricting-access-to-server-firmware-updates
title: HP Restricting Access to Server Firmware Updates
categories:
- Opinion
tags:
- hp
---

HP has announced that they will only provide firmware updates to customers with a valid warranty, Care Pack or support agreement. [HP says](http://h30507.www3.hp.com/t5/Technical-Support-Services-Blog/Customers-for-life/ba-p/154423#.UvbKSXn3q9V):

> We know this is a change from how we’ve done business in the past; however, this aligns with industry best practices and is the right decision for our customers and partners.
> 
> Our customers under warranty or support coverage will not need to pay for firmware access, and we are in no way trying to force customers into purchasing extended coverage.  That is, and always will be, a customer’s choice.

Two items concern me here. First is "this aligns with industry best practices", second is "...the right decision for our customers...".



## Good for Customers?



HP describes their firmware as "valuable intellectual property." But how many customers actually see it that way? The firmware is an integral component of the hardware - one cannot exist without the other. I can't use HP firmware in non-HP systems, nor can I operate HP servers without HP firmware. But I don't really get anything out of the firmware. I get my system up and running, and then hopefully the BIOS/UEFI just gets the hell out of the way.

{% include note.html content="Aside: One of the best things about virtualisation is that OS reboots now don't usually mean an interminable wait for the BIOS to do nothing of any apparent value" %}
We resent having to update firmware, especially with HP systems, where every individual hardware component seems to have its own firmware, down to each individual power supply. Until recently, you would be faced with multiple rounds of reboots to do all the updates, and you couldn't really show any true business value from it. Fixing bugs that shouldn't have shipped doesn't count as business value.

So now when we go to download updates we are going to have to ensure that those systems are under warranty, or support. What happens when those systems are under warranty, but HP's system doesn't agree? What about all the effort associated with making sure that the support lists are 100% accurate? What about when a reseller made a typo when updating their records? This is a non-zero cost to the business, but there is no value here - how is this the "right decision for customers"? What do we gain as customers?



## Industry "Best Practices"



Defining "best practices" is tricky. Let's look at what "standard practices" might be, by checking out some other major suppliers:




    
  1. [IBM^WLenovo](http://www.ibm.com/): They are going through the same process as HP. I tried to download firmware for the x3250 M4, and got this [![IBM Entitlement Validation](/assets/2014/02/IBM-Entitlement-Validation.png)](/assets/2014/02/IBM-Entitlement-Validation.png)

    
  2. [Dell](http://www.dell.com/): I went to Dell's website, and tried to download a [BIOS package for a Dell R620](http://www.dell.com/support/drivers/us/en/04/DriverDetails/Product/poweredge-r620?driverId=T2RVC&osCode=XI51&fileId=3329675946&languageCode=EN&categoryId=BI). No problems, no need to login, nothing - download was easy to find, and started streaming down.

    
  3. [Cisco](http://www.cisco.com/): Cisco's been cracking down on IOS downloads for a while now. Although I'm a CCIE, I don't get access to any downloads. I don't even get access to the Bug Toolkit, unless I have a support contract. But I just tried downloading "[Software for the UCS B-Series blade server products](http://software.cisco.com/download/release.html?mdfid=283853163&reltype=latest&relind=AVAILABLE&dwnld=true&softwareid=283655681&rellifecycle=&atcFlag=N&release=1.4(4l)&dwldImageGuid=9F4FFA0870D2A21ACC5BB04163DE6E5EBE2835DB)", and I was allowed to, once I had logged in. That account has no support contracts attached to it, so they're tracking downloads, but not stopping me. Yes, I was surprised by this too.

    
  4. [HP Networking](http://www.hp.com/go/networking): Lifetime warranty - HP Networking promises free software upgrades for as long as you own the product. They make quite a [song and dance about this policy](http://h30507.www3.hp.com/t5/HP-Networking/Lifetime-Warranty-2-0-Setting-a-new-industry-benchmark/ba-p/143541#.UvbS-EKSym0) even.





## So why are they doing it?



It's all about the 3rd-party support providers. Joe Blow can set up a support organisation for HP servers, and not bother going through official partner accreditation. Buy a few hardware spares, build up your own internal knowledge-base, and just grab the firmware from the HP site. Provides the support that most customers need, at a lower cost than official partners or HP direct.

Now those 3rd-parties will need to keep one example of each system nominally under support, so they can get updates, then re-distribute them. Presumably we'll see HP crack down harder on those organisations though, as they'll make sure their EULAs include language about "these updates are only to be used on systems under support by HP, blah blah".

It's not much different to Cisco's crackdowns on grey-market support providers. That led to much more onerous Cisco licensing procedures...hopefully server vendors don't start something like that.



## Do what you must, but be honest



HP can do as they please. They can charge whatever they like, and make us jump through all manner of hoops to download updates. But please don't insult us by telling us that it's the "right decision for customers." Be honest about your motives. This decision will mean extra work and hassle for already-stressed system administrators, and it won't give them a single thing in return. Unless maybe it's to give sysadmins another reason when they're telling the boss why that old G5 server needs replacing?
