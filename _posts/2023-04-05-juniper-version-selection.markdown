---
author: lindsay
categories:
- Routing &amp; Switching
date: "2023-04-05 20:00 -07:00"
layout: post
slug: juniper-version-selection
tags:
- Juniper
title: Juniper Version Selection
---

Picking the right Junos version is important. If you're not familiar with Juniper, finding and downloading the right software package is confusing. Here's some guidance on picking the right version.

{% include tip.html content="TLDR: Check the Suggested Releases page, find the latest service release in the suggested version for your platform. Almost never use the very latest version. Never use the version the box shipped with. Pay attention on the Downloads page, there are traps." %}

It's useful to understand Junos version numbering, and the upgrade policy. Then check the [Suggested Releases](https://supportportal.juniper.net/s/article/Junos-Software-Versions-Suggested-Releases-to-Consider-and-Evaluate?language=en_US) page to see what they recommend, check if that makes sense, and figure out how to get from here to there.

## Understanding Version Numbering

These days Juniper publishes a new release train every quarter. Versioning is simple "\<year\>.\<quarter\>.R\<release number\>". So 21.4R1 is released in the 4th quarter of 2021. New releases add new features and support new hardware. Configs may break

They then publish "service releases" on top of that, for example 21.4R1-S1 and 21.4R1-S2. These are supposed to only be bugfixes, but complacency breeds contempt. So sometimes they throw in throw in breaking changes that may render your existing config non-bootable, because why the hell not? Be grateful if they're documented, like the [payload-protocol vs next-header](https://supportportal.juniper.net/s/article/21-4R2-S1-EVO-IPv6?language=en_US) change.

A few months later they publish an "R2" release that rolls up bugfixes, and may have some small changes in behavior. No more service releases to the R1 train after that. A few more service releases, then they introduce an R3 release. Again rolls up bugfixes, perhaps with small behavior changes. They might add some more service releases on R2 after R3 comes out. I wish they wouldn't.

The R3 release will see service releases over the course of its lifetime, becoming further and further apart. No R4. Many engineers look for R3 releases before considering upgrades.

{% include tip.html content="Pre 2017 versions, e.g. 12.x and 15.x had their own thing going on with release numbering, but you can ignore that if you're working on supported hardware." %}

### Extended End of Life

Juniper went a bit funny with "Extended End of Life" versions for a while. Their docs were full of references to those versions, but they were all years out of date. I couldn't predict which versions would be considered extended. They fixed this in the last couple of years. Now it's obvious - the "even" numbered releases such as 21.2, 21.4 are "Extended" - they have 3 years of Engineering support after first release. The odd-numbered releases like 21.1 and 21.3 have 2 years of support.

This is 3 years from the first R1 release. Given that the R3 release comes out around 9 months after R1, and you'll wait for a couple of service releases on top of that, it's often a year from release when you install it. So you'll want to be on the extended end of life releases, to give yourself a couple of years of support.

The [Junos OS Dates & Milestones](https://support.juniper.net/support/eol/software/junos/) is your go-to page to check support lifecycles. Some releases will get even longer support, e.g. for specific EoL hardware.

## JTAC Suggested Releases

Juniper has a great page "[Junos Software Versions - Suggested Releases to Consider and Evaluate](https://supportportal.juniper.net/s/article/Junos-Software-Versions-Suggested-Releases-to-Consider-and-Evaluate?language=en_US)." That page has a section for each of the main product lines, and model-specific guidance. Find your model, and check the listed versions, and "last updated" date.

{% include note.html content="They used to call it \"Recommended\" versions rather than \"Suggested,\" toning it down a little. It's still your first and best place to start when picking a version." %}

For example, if you're updating an EX3400, it says "Latest 21.4R3-Sx", last updated 14 Nov 2022. Straightforward - find the latest available service release for the 21.4R3 release train. Last updated a few months ago, so it's solid advice.

Yes, these versions will still have bugs. There's no guarantee they are perfect. But if you don't have any specific requirements, they are always a solid choice. They are mostly conservative, but there will be no surprises from JTAC if you log a case against them. They will almost always be extended end of life releases, except for specific hardware support.

You should bookmark that page, and sign up for change notifications.

## OK, But What About <?>

What if I don't like the suggested release? What if I need feature \<X\>? Or what if they have multiple suggested versions, e.g. for MX80 today they suggest 20.2R3, 20.4R3, 21.2R3. Not helpful.

In those cases you'll need to make your own decision. Remember that Juniper is offering suggested releases - as long as you are still running a supported release, they will still support you. They might question your choice, but if you have a solid rationale, it's fine.

If you really need feature <X> that is only in the very latest code, that's what you have to do. If running brand new hardware, you might need a brand-new software release.

My general advice in those situations is:

* Pick an even-numbered release if possible
* Pick the oldest even-numbered version that has the features you need
* Take the latest service release in that train

If there are multiple suggested versions, look at the future support lifetime, and pick something you're comfortable with. E.g. 20.2R3 is about to go end of support. That's a poor choice if you're upgrading today. In my case I would pick the newest branch, unless I had wide deployment of an older branch and was sticking with it for a while.

## What's this about hardened releases?

> I saw a reference to hardened releases somewhere - what about those?

My advice is to ignore those. As best as I can tell, Juniper decided to do "hardened" releases for specific use-cases, e.g. EVPN-VXLAN, or VCF. But they did a poor job of explaining what a hardened release is, and have not kept their documentation up to date. You can see traces of it in this [PDF](https://pathfinderstatic.s3-us-west-2.amazonaws.com/public/DC-Architecture-Hardened-release-recommendation.pdf), but I have not seen any clear public definitions. If you go to Pathfinder and click on [Data Center](https://apps.juniper.net/home/segment?segment=Cloud%20Providers&subSegment=Data%20Center), then select an architecture, e.g. [MC-LAG](https://apps.juniper.net/home/architecture?type=Data%20Center%20Architectures&name=mc-lag&segment=Cloud%20Providers&subSegment=Data%20Center), you'll see a "hardended release." At the time of writing, it says the hardened release is 20.2R3. Installing a release today that goes End of Engineering in June this year is a bad move. Don't do that.

The "Suggested Releases" page has recommendations for use-cases for the QFX platforms. At 2023/03/30, for MC-LAG (QFX5K) it says "Latest 21.4R3-Sx / 22.2R3". Those are better choices. At the time of writing, they suggest the same version for all use-cases. Probably tells us something about what happened to their plans for "hardened" releases.

## Juniper Upgrade Policy

> If you're trying to get to there, I wouldn't start from here

When I upgrade a standalone Arista switch, I copy the new software to it, tell it to use that image, and reload. I don't care what version I'm on today. With Junos, your current version impacts how you get to your destination. Depending on your current version, you might officially need to go via interim steps to get to your target.

The [official policy](https://www.juniper.net/documentation/us/en/software/junos/release-notes/21.3/junos-release-notes-21.3r1/topics/upgrade-downgrade/ex-upgrade-downgrade.html) says:

> For both EOL and EEOL releases, you can upgrade to the next three subsequent releases or downgrade to the previous three releases. For example, you can upgrade from 20.4 to the next three releases – 21.1, 21.2 and 21.3 or downgrade to the previous three releases – 20.3, 20.2 and 20.1.
>
> For EEOL releases only, you have an additional option - you can upgrade directly from one EEOL release to the next two subsequent EEOL releases, even if the target release is beyond the next three releases...For example, 20.4 is an EEOL release. Hence, you can upgrade from 20.4 to the next two EEOL releases – 21.2 and 21.4

If you do annual upgrades, it's easy. Going 20.4 to 21.4? Straight shot. What if you have 18.4? Do you really want to go 18.4 -> 19.4 -> 20.4 -> 21.4? Tedious. What if you've found an old box on the shelf and want to use that? Junos upgrades and reboots are very slow, do I have to go through all that? Can't I skip some steps?

If you do a USB/TFTP install, you can go straight to any version. If you're doing this online...it is very version- and platform-dependent. I have my own experience for what steps I can do. For example, I know I can take a QFX5110 from 18.4R2 -> 21.4R3 in one step. I also know that EX3400 18.4R2 -> 20.4 is impossible if it's too early a service release on the EX3400. I've hit similar issues on MX, where some steps were too large. If you have a large fleet to upgrade, try it out on a some low risk systems. Generally it fails at install time. If you have a handful of systems, then sticking to the guidance is safer.

Some platforms specifically call out supported large jumps, e.g. SRX 15.1 -> 19.4R3. If Juniper says it's OK, then go for it. Otherwise test first.

## Downloading the right software

OK, we've figured out what version we want, and any interim releases we need. Let's download it. Should be simple right? Yes and no. It's easy once you know the tricks, but hard at first.

Let's say we have a QFX5100, and we want to run the latest 21.4R3-Sx release, as suggested.

Start with the [Downloads](https://support.juniper.net/support/downloads/) page, and put QFX5100 into the search box:

[![QFX5199 Download](/assets/2023/04/qfx5100_download.jpg)](/assets/2023/04/qfx5100_download.jpg)

uh...do I want QFX 5e Series Switch, QFX 5e Series Switch with Enhanced Automation, Limited - MacSec Enabled QFX 5 Series, Limited - QFX 5 Series Switch with Enhanced Automation, QFX 5 Series Switch, QFX 5 Series Switch with Enhanced Automation?

Turns out you don't want any of those. The first page of results is a trap. It only has the "R" releases. You almost never want a base "R" release, you want a Service Release, e.g. R3-S4.

Scroll up, go to the drop-down, select "Junos SR", and make sure 21.4 is the selected version. Now we see the service releases, in reverse chronological order. 21.4R3-S3 is the latest at release time. But there's 6 variations. Which one do I want?

As a general rule, you never want a "Limited" release unless you're in specific restricted countries. If you're in one of those places you'll know about this, if you'ver never heard of it you can ignore it. So now the choices are 5 vs 5e, and with or without "Enhanced Automation." If you don't know, choose "Enhanced Automation" - it will help you later. Read [this](https://www.juniper.net/documentation/us/en/software/junos/automation-scripting/topics/concept/junos-flex-overview.html) for more info. Last thing, 5 vs 5e? For QFX5110 and QFX5120, you must use the "5e" image, and it will be the only option you see. For QFX5100 you can install 5 or 5e images. This is a trap. Do not install 5e unless you know exactly why you're doing it. Get the "5" image. 

Other products like the PTX10001-36MR are much simpler. There are just one or two download variations.

One last thing before downloading that file. Make sure you are selecting the file from the "Install Package" section

[![QFX5199 Download](/assets/2023/04/install_package.jpg)](/assets/2023/04/install_package.jpg)

Do not download the file from the "Install Media" section unless you are trying to create a USB image. For a while the default page for some products opened to the Install Media section. Made me download the wrong file quite a few times.

### What about Junos vs Junos Evolved?

Go to the downloads page for PTX10003, and you'll get this dropdown:

[![PTX10003 Download](/assets/2023/04/ptx10003_download.jpg)](/assets/2023/04/ptx1003_download.jpg)

Can I choose if I want Junos Artisanal or Junos Evolved? No, this is just an artifact of the way they publish OpenConfig models. Those are listed under Junos, but the software you want to download is under "Junos Evolved SR." Do not ask me why.

Once you've finally tracked down the right software, click on the package, login if required, accept the license, and download it. Then copy it to your switch, and install it. Exact methods vary by platform, start [here](https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade/topics/topic-map/install-software-on-ex.html#id-installing-software-on-an-ex-series-switch-with-a-single-routing-engine-cli-procedure) for common methods. Install it, reboot, and cross your fingers!