---
author: lindsay
date: 2014-06-28 01:28:40+00:00
layout: post
slug: cloudflare-that-was-easy
title: 'CloudFlare: That Was Easy'
categories:
- Security
tags:
- IPv6
---

I switched this blog over to using [CloudFlare](http://www.cloudflare.com/) a few days ago. It's all been pretty painless, and I highly recommend it to others.



## What is CloudFlare, and Why Use It?



CloudFlare "[protects and accelerates any website online.](https://www.cloudflare.com/overview)" It does this by acting as a reverse proxy, sitting between end-users and your website. All traffic to your website gets directed through the CloudFlare global network. Because CloudFlare sees all traffic to your site, it can inspect it, dropping malicious traffic, and only passing valid requests on to your site.

CloudFlare also caches content, improving performance for your users, and reducing load on your web server. There are some other features you can enable, such as redirecting mobile users to a mobile-specific site, enabling SSL, etc. CloudFlare offers a range of features and pricing plans, but at the moment, I only need the features provided at the free level.

The key feature that attracted me was IPv6 - CloudFlare can act as an IPv6 to IPv4 gateway, providing native IPv6 access to end-users, even when your hosting provider can't yet provide native IPv6. Perfect!

The other reason I finally got around to moving to CloudFlare was that Tim Hoffman [has recently joined them](http://blog.hoff.geek.nz/2014/05/06/cloudflare-san-francisco-and-the-next-chapter-of-life/) - this was the trigger I needed to complete the setup.



## Migration Steps:



Here's the steps I went through to add my blog to CloudFlare:

  1. Create an account at CloudFlare, and add lkhill.com - it then scanned my domain, and chose some sensible defaults - e.g. noting that I have ftp.lkhill.com, and configuring that to bypass CloudFlare.

  2. Updated the NS records for lkhill.com to point to the DNS servers provided by CloudFlare

  3. [Add PageRule](https://support.cloudflare.com/hc/en-us/articles/201717894-Using-CloudFlare-and-WordPress-Five-Easy-First-Steps) to exclude `/wp/wp-admin/*` from Caching.

  4. Install [CloudFlare Wordpress plugin](http://wordpress.org/plugins/cloudflare/)

  5. Change CloudFlare settings to enable IPv6 (CloudFlare Settings -> Automatic IPv6 -> Full)


Pretty painless really.


## Profit! (?)



Now I can look at some pretty analytics like this:

[![CloudFlare Analytics](/assets/2014/06/CloudFlare-Analytics.jpg)](/assets/2014/06/CloudFlare-Analytics.jpg)

{:.image-caption}
CloudFlare Analytics

Note how it's saying it stopped an attack from New Zealand - I wish it could tell me who it was, so I can go around to their house and sort them out. Because of course everyone knows everyone in this country.

I can also see the volume of requests & traffic saved by CloudFlare - this is a not-insignificant amount over the last week:

![CloudFlare Traffic Saved](/assets/2014/06/CloudFlare-Traffic-Saved.png)

I'm looking forward to seeing the stats over a longer timespan. I'd also like to explore using the CloudFlare API to pull statistics into some of the other monitoring platforms I work with. A challenge for another day.

If you're running your own website, you should investigate CloudFlare. It will improve your security and availability, and give you IPv6 access, even if your hosting provider is dragging their feet. Recommended.

{% include note.html content="NB: I have no affiliation with CloudFlare. I'm just a happy customer, using the same free tier available to everyone" %}

