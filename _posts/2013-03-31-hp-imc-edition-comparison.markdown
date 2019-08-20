---
author: lindsay
date: 2013-03-31 01:55:00+00:00
layout: post
slug: hp-imc-edition-comparison
title: HP IMC - Edition Comparison
categories:
- HP IMC
tags:
- hp
- imc
---

[HP IMC](https://www.hpe.com/networking/imc) was originally offered in two editions - [Standard](https://www.hpe.com/us/en/product-catalog/networking/networking-software/pip.hp-intelligent-management-center-standard-software-platform.4176535.html) and [Enterprise](https://www.hpe.com/us/en/product-catalog/networking/networking-software/pip.hp-intelligent-management-center-enterprise-software-platform.4176520.html). As of v5.2, there is also a "[Basic](https://www.hpe.com/us/en/product-catalog/networking/networking-software/pip.hp-intelligent-management-center-basic-software-platform.5333786.html)" edition. The new "Basic" edition is feature-limited, aimed at smaller customers with limited network management requirements.

**[UPDATE April 13 2015: Prices and node counts have been updated]**

The main goal of Basic is to put [PCM+](http://h17007.www1.hp.com/us/en/networking/products/network-management/HP_PCM_Plus_Network_Management_Software_Series/index.aspx) out to pasture. It comes in two flavours:

* [IMC Basic Software Platform](https://www.hpe.com/us/en/product-catalog/networking/networking-software/pip.hp-intelligent-management-center-basic-software-platform.5333786.html)
* [IMC Basic WLAN Manager Software Platform](https://h17007.www1.hp.com/us/en/networking/products/network-management/IMC_WLANM_Software/index.aspx) - same as Basic, with added Wireless Management capabilities

There is plenty of information published on each edition, but HP does not seem to have a table showing a side-by-side comparison of the editions. This can make it a little hard to work out which edition you should choose. Here's my stab at bringing this information together:

|----------+----------+----------+----------|
|Feature   | Basic    | Standard |Enterprise|
|----------+----------+----------+----------|
|**Managed SNMP Nodes**|50 (Max)|50 (base, expandable)|50 (base, expandable)|
|----------+----------+----------+----------|
|**Managed Wireless APs**|50 with Basic WLAN Manager Expandable.|0 base, need license for WSM|0 base, need license for WSM|
|----------+----------+----------+----------|
|**Fault Management**|Yes|Yes|Yes|
|----------+----------+----------+----------|
|**Performance  Mgmt**|Yes|Yes|Yes|
|----------+----------+----------+----------|
|**Reporting**|Yes (Basic)|Yes (Basic, custom reporting requires iAR license)|Yes (Basic, custom reporting requires iAR license)|
|----------+----------+----------+----------|
|**Config Mgmt**|Basic, no compliance mgmt|Yes|Yes|
|----------+----------+----------+----------|
|**WLAN Monitoring**|Yes with WLAN version|With WSM license|With WSM license|
|----------+----------+----------+----------|
|**NetFlow/sFlow**|sFlow only, max 24 hours data|0 base, can add NTA license|5 nodes base, can add more with NTA license|
|----------+----------+----------+----------|
|**Embedded DB**|Yes, Windows only|Yes, Windows only|No|
|----------+----------+----------+----------|
|**External DB**|Yes - SQL Server, MySQL, Oracle|Yes - SQL Server, MySQL, Oracle|Yes - SQL Server, MySQL, Oracle|
|----------+----------+----------+----------|
|**Syslogs**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Service Monitor**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Security Control Centre**|No|Yes|Yes|
|----------+----------+----------+----------|
|**ACL Management**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Virtual Connect**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Customised Functions**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Virtual Network Mgmt**|No|Yes|Yes|
|----------+----------+----------+----------|
|**Hierarchical Monitoring**|No|Can be lower tier|Yes|
|----------+----------+----------+----------|
|**eAPI**|No|With license|Yes|
|----------+----------+----------+----------|
|**Additional Modules Available**|None|UAM, NTA, EAD, VSM, QoS, MPLS, UAM, SHR, IPSec VPN, WSM, eAPI, TAM, APM, VAN, RSM, iAR|UAM, EAD, VSM, QoS, MPLS, UAM, SHR, IPSec VPN, WSM, TAM, APM, VAN, RSM, iAR|
|----------+----------+----------+----------|
|**Target Market**|Small HP wired/wireless networks. PCM+ replacement.|Most medium-sized businesses, that don't need hierarchical monitoring.|Large, distributed networks with many sites, needing hierarchical monitoring|
|----------+----------+----------+----------|
|**Price (at cdw.com)**|[$1,529.99](http://www.cdw.com/shop/products/HP-Intelligent-Management-Center-Basic-Edition-license/2962801.aspx), or [$4,221,99](http://www.cdw.com/shop/products/HP-IMC-BSC-WLAN-MGR-PLTFM-50/2985685.aspx) (WLAN version)|[$3,066.99](http://www.cdw.com/shop/products/HP-Intelligent-Management-Center-Standard-Edition-license/3147343.aspx), additional 50 nodes [$2,299.99](http://www.cdw.com/shop/products/HP-Intelligent-Management-Center-Standard-and-Enterprise-license/3147346.aspx)|[$11,499.99](http://www.cdw.com/shop/products/HP-Intelligent-Management-Center-Enterprise-Edition-license/3160040.aspx), additional 50 nodes [$2,299.99](http://www.cdw.com/shop/products/HP-Intelligent-Management-Center-Standard-and-Enterprise-license/3147346.aspx)|
|----------+----------+----------+----------|

---

Note that you can get upgrade pricing from PCM+ to IMC.

{% include warning.html content="The inability to expand the Basic edition beyond 50 SNMP nodes, along with limited configuration management capabilities, means it should only be considered for small networks, that are extremely unlikely to grow. I don't know if Basic customers can upgrade to Standard or Enterprise." %}

{% include note.html content="This information was collected from publicly available sources. Do not take it as official - there may be some things I have missed. Errors will be updated as I become aware of them. Prices were obtained from CDW.com on April 13, 2015, and are in USD.  I've published these prices for comparative purposes - you can probably get a bigger discount, particularly if you're buying HPN kit." %}
