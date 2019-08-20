---
author: lindsay
date: 2014-11-01 23:30:39+00:00
layout: post
slug: in-praise-of-support-lifecycles
title: In Praise of Support Lifecycles
categories:
- Opinion
tags:
- software
- support
---

If you're just starting out working with 'Enterprise' products, you may not have come across Support Lifecycles. It's important to know what these are, and how it affects you. They can have both a positive & a negative impact on when and why you choose to upgrade systems.

## What Are Support Lifecycles?

Developers would like to only support the latest version. But customers can't/won't always run the latest version. They need to know that they can expect a certain level of support for the version they're running. As a compromise, software vendors will publish a support lifecycle policy. This will outline the levels of support a product gets, from new product introduction, through to being superceded, and finally moved to end of support. Typical phases include:

* General Support: Product is in General Availability phase, and is fully supported. You can log support cases, search KB articles, and expect both functionality enhancement and bugfix patches. The current product version will always be in this phase, and typically 1-2 major versions behind will also be included.
* Limited Support: You can log a support case, and we'll try to help, but we're not planning any new patches, and you'll probably get a suggestion to upgrade. You might get lucky and get critical security patches, but don't count on it. Older software versions get moved here as they are superceded.
* End of Support: You can still search through the KB yourself, but don't ask us for help: We'll tell you to upgrade.

Vendors do vary slightly in their approaches, and it's worth knowing the differences. You need to know how long you can expect a new product to be supported for, and what happens as it moves out of General Support. Here's some examples for [Cisco](http://www.cisco.com/c/en/us/products/eos-eol-policy.html), [VMware](https://www.vmware.com/support/policies/lifecycle), [Microsoft](http://support2.microsoft.com/lifecycle/default.aspx?LN=en-us&x=5&y=11), [Check Point](http://www.checkpoint.com/support-programs-and-plans/support-lifecycle-policy/index.html).

You will be able to look up the current status of various product versions - e.g. VMware p[ublishes this matrix](https://www.vmware.com/files/pdf/support/Product-Lifecycle-Matrix.pdf). Note that sometimes the End of Support date for a product is not yet known. This might be because they don't have firm plans for the next version yet. In those cases they'll probably give you a minimum length of time the product will be supported for - it can always be extended.

Any good software vendor should be publishing this information freely to all customers. You should not need a support contract to find this information. As such, there is no excuse for getting caught out when products you use go end of support.

## Is it an excuse to force me to upgrade?

Yes and no. Vendors need to choose how to focus their efforts, and can't be expected to support old software forever. They don't want to maintain huge numbers of code branches - at some point customers have to upgrade, or lose support. It's not unreasonable to expect to upgrade when the vendor has since released several new versions. Of course, you don't have to upgrade: You can continue using your existing featureset.

It is a problem if you need to upgrade too frequently. That's why 'Enterprise' software companies usually commit to a minimum support period, often 3-5 years. Consumer software companies will have a much shorter support lifecycle.

But you can also use these policies to your advantage - either to get the vendor to properly support your version, or to help you get internal buy-in for a planned upgrade.

Let's say you're having a problem with version 5.x, and the latest version is 6.x. The default response from Support is often "Upgrade to 6.x." But if you can point to the official support policy which says that 5.x is still fully supported, you can force the vendor to properly support your version.

Or imagine you're running ESX 4.1. You've been wanting to upgrade to ESXi 5.5 for a long time, because you want to be able to use the web client from your Mac. But you haven't been able to get the internal buy-in you need. "Why should we upgrade? 4.1 does everything we need!" Well, earlier this year 4.1 went End of General Support. Now you can add this to your upgrade business case "We need to stay on a fully supported platform."

## Line in the Sand

There's one other situation where support lifecycles are useful - when you're indirectly affected by them. Imagine you're running a popular website, and you're trying to do your best to maintain cross-browser compatibility. It's getting harder and harder to support browsers that are more than 10 years old. There's a lot of amazing new features you want to use, but it's hard to gracefully degrade with old browsers that don't support them. This isn't just functionality either - it might be security-related, such as wanting to drop SSLv3 support.

If you had a core of clients that still use Windows XP with Internet Explorer, you couldn't completely ignore them. Clients could say "Look, it's still a Microsoft-supported platform." But now that Microsoft's ended XP support, users can't keep using that line. They can keep using XP, and they can complain that your site doesn't properly support them, but you've got the backing to say "That platform is no longer supported. You need to upgrade to something written within the last 5 years."

Cutting off old versions means lets us do things like code websites to modern standards and enforce higher minimum security settings. This improves the overall health & stability of the Internet. Support Lifecycles can act as a line in the sand - it might mean you drag out support longer than you'd like, but once that line is crossed, you can move on. Never-ending backwards compatibility comes at a huge cost in time and effort. At some point you need to cut those old systems adrift. Could CloudFlare have [disabled SSLv3 globally](http://blog.cloudflare.com/sslv3-support-disabled-by-default-due-to-vulnerability/) if Windows XP was still supported?

Know what the support policies are for your key software. Understand why they exist, and figure out how to use them to your advantage.
