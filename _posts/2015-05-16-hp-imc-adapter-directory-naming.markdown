---
author: lindsay
date: 2015-05-16 05:48:23+00:00
layout: post
slug: hp-imc-adapter-directory-naming
title: HP IMC Adapter Directory Naming
categories:
- HP IMC
tags:
- hp
- imc
---

This week's lesson: Be consistent with your vendor naming when working with [HP IMC](https://www.hpe.com/networking/imc/) Custom Device Adapters. When you create the new adapter directory, use exactly the same vendor name as used within the UI. Otherwise IMC may not recognise your new adapter. Case matters too, even on Windows!

HP IMC ships with a set of "Device Adapters" that define functions such as backups, configuration deployment, firmware upgrades, etc. These adapters are sets of XML, TCL and Perl files. They define which devices are supported, for what functions, and how to execute those functions.

Obviously HP can't support every device ever made. But they're quite happy for you to write your own adapters, or extend the ones they have. So if you've got a few unsupported switches, and they have some sort of sensible interface, you can write your own adapters.

These are stored at `<IMC>/server/conf/adapters/ICC/`. Under there, you have a set of folders for each vendor. Under each vendor folder is an `adapter-index.xml` file, which maps SNMP sysOIDs to adapters. You must have a mapping in the `adapter-index.xml` file for your sysOID. (nb you can use wildcards). If those XML files change, you need to restart IMC.

I had thought that the key part was the mapping in the `adapter-index.xml` file. But IMC also looks at the vendor name. In the main IMC UI, you can define models/series/vendors, and SNMP sysOIDs to those models. IMC will use this vendor name to decide which adapter folder to look in. It will then use the `adapter-index.xml` mappings to pick out the specific adapter type.

So if you're adding a new adapter to IMC, follow these steps:

1. Go to Settings -> Device Vendor. Make sure the vendor is there. Same with Device Series
2. Add a mapping for the specific Device Model - e.g. 1.3.6.1.4.1.2636.1.1.1.2.64 is a Juniper SRX-110
3. Create the new adapter folder under `conf/adapters/ICC/`. This folder name must match the vendor as defined in the UI.
4. Create your new adapter files, including the correct mappings in `adapter-index.xml`
5. Restart IMC. Re-synchronise your device. Check that it displays the right model name in IMC. It should now try and use your new adapter for backups, etc.

Sometimes you're having problems figuring out which adapter IMC is using, or if it's even been able to figure out any adapter at all. I've found these tables in the database are helpful:

```sql
mysql> use config_db;
Database changed
mysql> select dev_id,dev_ip from tbl_dev where dev_ip = '192.168.1.1';
+--------+-------------+
| dev_id | dev_ip      |
+--------+-------------+
|     13 | 192.168.1.1 |
+--------+-------------+
1 row in set (0.00 sec)

mysql> select * from tbl_dev_adapter where dev_id=13;
+----------------+--------+-------------+-----------------------------+-----------------+--------------+------------+
| component_name | dev_id | vendor_name | sysoid                      | adapter_name    | adapter_type | error_code |
+----------------+--------+-------------+-----------------------------+-----------------+--------------+------------+
| Custom         |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | N/A             |            0 |         18 |
| Custom         |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | N/A             |            1 |         18 |
| ICC            |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | DlinkGeneric    |            0 |          0 |
| ICC            |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | N/A             |            1 |         44 |
| netresdm       |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | N/A             |            0 |         18 |
| netresdm       |     13 | D-Link      | 1.3.6.1.4.1.171.10.137.3.1  | N/A             |            1 |         18 |
+----------------+--------+-------------+-----------------------------+-----------------+--------------+------------+
6 rows in set (0.00 sec)

mysql>
```

There I'm looking up the device ID, then checking the `tbl_dev_adapter` table to see what it's using. The key there is **DlinkGeneric**. If you don't get any results here, IMC can't figure out the device adapter mapping.

Hopefully this helps someone else who's scratching their head about why IMC doesn't recognise their new adapter.
