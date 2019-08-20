---
author: lindsay
date: 2013-12-09 16:17:48+00:00
layout: post
slug: help-imc-doesnt-support-my-new-cisco-switch
title: 'Help: IMC Doesn''t Support My New Cisco Switch!'
categories:
- HP IMC
tags:
- cisco
- imc
---

I've recently been asked about using IMC to backup newer Cisco devices, such as the 4500X, or 3850. HP has not yet validated the backup process for these devices, so it's not officially supported by IMC. But you and I know that a backup for a new IOS-based device looks pretty much the same as an older one, so surely we can just tell IMC to re-use the same backup scripts it already has?

Sure can. It's a reasonably easy 4-step process:

1. Find the sysObjectID for your device
2. Ensure IMC has the Device Series + Model definition added to the GUI
3. Update the Cisco adapter-index.xml file.
4. Synchronise the device

Then re-run the backup, and fingers crossed, it should work.

Here's how:

## Finding the sysObjectID

All SNMP-capable devices respond to a request for their sysObjectID with a code that should identify that specific switch model. If you've already added the device to IMC, just go to the Device Details page for that device, and look for the `sysOID` field - it should look something like `1.3.6.1.4.1.11.2.3.7.11.86` Alternatively, if you have the net-snmp tools installed, you can retrieve it with `snmpget -v2c -c X.X.X.X system.sysObjectID.0`

IMC will use this string to map the correct backup adapter.

## Add the Device Series/Model

You may have found your device is listed as something like "Cisco Generic Switch." If it does not appear correctly in IMC, go to System -> Resource Management -> Device Series. Try doing a partial-match search for the series name - e.g. if you're trying to get a Cisco 3850 added, you might look for "3850." If the right series is already defined, go straight to adding the new model. Otherwise, click "Add" and set the Series Name and Vendor as required. In this case, you might add "Cisco Catalyst 3850"

Then go to System -> Resource Management -> Device Model. Now we can add the individual models - e.g. 3850-48P-S, 3850-24P-S, etc. For each model you want to add, click Add, and fill in the necessary details. You can either paste in the sysOID, or click "Query Device" and you can get IMC to fill in the details from a device already added to IMC. Don't forget to set the Category to something appropriate.

If you're adding support for the Cisco 3850, you might add these four models:

* Cisco 3850-48P-E Switch - 1.3.6.1.4.1.9.1.1641
* Cisco 3850-24P-E Switch - 1.3.6.1.4.1.9.1.1642
* Cisco 3850-48P-S Switch - 1.3.6.1.4.1.9.1.1643
* Cisco 3850-24P-S Switch - 1.3.6.1.4.1.9.1.1644

So far this is mostly cosmetic. Now we're going to tell IMC which adapter to use.

## Update Cisco/adapter-index.xml

Login to your IMC server via RDP or SSH, and go to `[IMC Dir]/server/conf/adapters/Cisco/`. Edit `adapter-index.xml`. Look for this section:

```text
 <adapter name="CiscoIOSL3Switch">
            <description>Cisco switches, Catalyst 2950, 2970, 3550, 3750 and 8500 series, IOS version 12.x</description>
            <sysoid>1.3.6.1.4.1.9.1.368</sysoid>
            <sysoid>1.3.6.1.4.1.9.1.367</sysoid>
            <sysoid>1.3.6.1.4.1.9.1.431</sysoid>
            <sysoid>1.3.6.1.4.1.9.1.366</sysoid>
            <sysoid>1.3.6.1.4.1.9.1.452</sysoid>
...
```

This section continues on, listing all the supported devices. Go to the end of that section, and just before it closes, add new lines with your new sysOIDs. Save and close the file, and restart IMC. IMC usually needs a restart if you're changing XML files.

## Synchronize the Device

IMC may have cached the wrong backup adapter for your device. From the Device Details page, click on "Synchronize". Amongst other things, this will tell IMC to re-check which backup adapter should be used. You will need to do this for all examples of the device that were added to IMC before you changed adapter-index.xml. Bulk synchronization can be done from either a Device View, or with the results of a search. Select all devices to update, then click Synchronize.

## Re-run the backup

Run the backup manually, and wait for it to complete. All going well, it should now complete successfully. If not, you'll need to dig into the logs to find out why.

I have uploaded a copy of [adapter-index.xml to GitHub](https://github.com/NetOpsCommunity/imc-netops/blob/master/adapters/ICC/Cisco/adapter-index.xml) that includes support for 2960S switches. If you have any other sysOIDs that you've tested out, let me know and I'll merge the changes.

Of course, this is not supported by HP. On the odd chance that the Cisco device needs a different backup script, we can probably work around that too. You might need to figure that bit out yourself (or pay for my time :D).

{% include warning.html content="**Note:** these changes may be lost when you upgrade IMC. Take a backup of adapter-index.xml beforehand, and manually merge your changes. Future upgrades will probably add the necessary sysOIDs anyway." %}
