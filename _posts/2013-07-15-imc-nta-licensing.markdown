---
author: lindsay
date: 2013-07-15 05:00:36+00:00
layout: post
slug: imc-nta-licensing
title: IMC NTA Licensing - Watch the Fine Print
categories:
- HP IMC
tags:
- imc
- netflow
---

HP IMC has an optional add-on module for NetFlow/sFlow Analysis called "[Network Traffic Analyzer (NTA)](http://h17007.www1.hp.com/us/en/networking/products/network-management/IMC_NTA_Software/index.aspx)." This comes in 10-, 20- or 50-node license packs. Based on the license names, you might think that it's as simple as counting up the number of NetFlow-generating devices in your network, and purchasing sufficient licenses. Not quite so fast.

[**UPDATE 10/10/13: This licensing policy has changed - as of IMC 7.0, it is purely node-based licensing. The restrictions highlighted here have been removed. Well done HP for seeing some sense.]**

Turns out that it is not only licensed on number of nodes, but also on the volume of traffic. Each NTA node license counts for 5Mb/s of traffic. This is cumulative, and the traffic distribution can be unequal. So if you have a 10-node license, you're allowed to report on up to 50Mb/s of traffic. That could be all from one node, or it could be evenly distributed across 10 nodes. If you're using more than your licensed rate, you need to purchase more node packs - even if you don't actually need any more nodes.

Note that the data rate that counts is the volume of traffic that NetFlow is reporting on - not the actual NetFlow data itself. So if you've configured a router to collect NetFlow data on all traffic flowing across it, licensing will be based on the volume of traffic through the router. But if you use sampling, then the license count is based on the sampled data. So if you've got 100Mb/s going through your router, and you're sampling 1 packet in 100, you'll be using around 1Mb/s of your licensing.

HP publishes more information about this licensing, along with some worked examples here: "[HP IMC Network Traffic Analyzer (NTA) License and Device Sampling Rate Explained](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?lang=en&cc=us&taskId=120&prodSeriesId=4197910&prodTypeId=329290&objectID=c03179342)." I strongly recommend reading this document if you're planning on using IMC to analyse NetFlow data. I particularly like this quote:

> It is difficult, if not impossible, to predict how much NTA traffic speed is likely to be in a given network

Errr...so why do you license it that way then?

The actual setup of of NTA is reasonably easy, except for a couple of tiny bits that can really frustrate you. Save yourself the pain - just watch this video on NTA configuration. It will make your life a lot easier.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-TG8fpbxRv8?ecver=1" frameborder="0" allowfullscreen></iframe>

{% include note.html content="For reference, HP NNMi NetFlow analysis (iSPI Traffic) is licensed on the number of interfaces generating data, and the type of flow data (NetFlow is more expensive than sFlow). Solarwinds NetFlow Traffic Analyzer needs to have a license that matches your NPM license - so it assumes that all nodes you have managed could be generating flow data." %}
