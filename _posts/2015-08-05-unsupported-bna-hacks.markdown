---
author: lindsay
date: 2015-08-05 05:25:13+00:00
layout: post
slug: unsupported-bna-hacks
title: Unsupported BNA Hacks
categories:
- NMS
tags:
- BNA
- Brocade
---

Here's a couple of quick hacks for working with [Brocade Network Advisor](http://www.brocade.com/en/products-services/network-management/brocade-network-advisor.html). It's unsupported, but you can run BNA on Ubuntu. You can also suppress the client-side JRE version mismatch warning.

{% include warning.html content="These are both completely unsupported by Brocade. Do not be surprised if it does not work as expected, and do not log a TAC case about it. These are provided for informational purposes only. If it breaks, you keep the pieces." %}

## Ubuntu Install

If you try to install BNA on Ubuntu, it fails during the DB initialization & setup phase. There are two reasons for this:

* `gawk` is not where the installer thinks it should be
* Some scripts run as `/bin/sh`, but use bashisms.

Before running the installation, make these two changes:

* Run `sudo ln -s /usr/bin/gawk /bin/gawk`
* Run `sudo dpkg-reconfigure dash` and select "No"

After that the DB setup will complete. Leaving the `gawk` symlink in place won't hurt anything else. You can _probably_ change the system shell back to dash, but you may run into problems if you run any of the BNA utility scripts.

## Client-side JRE check

When you launch the BNA Desktop client, it checks your local JRE version against a list of supported versions. It's reasonably up to date, but Java is a fast-moving target. So you might be running a new JRE, and you get this popup:

[![java version warning](/assets/2015/08/java-version-warning.png)](/assets/2015/08/java-version-warning.png)

To stop this message, edit the downloaded `dcmclient.jnlp` file. Search for "versions"

```xml
    <property name="jnlp.dcm.java.util.Arrays.useLegacyMergeSort" value="true"/>
    <property name="jnlp.dcm.supported.java.versions" value="1.7.0_79,1.7.0_80,1.8.0_45"/>
    <property name="jnlp.dcm.dcm.jmsuser.name" value="jmsuser"/>
```

Add your current JRE version:

```xml
    <property name="jnlp.dcm.supported.java.versions" value="1.7.0_79,1.7.0_80,1.8.0_45,1.8.0_51"/>
```

This will stop that popup. Note that this gets over-written if you download a new `dcmclient.jnlp` file, and/or upgrade BNA.
