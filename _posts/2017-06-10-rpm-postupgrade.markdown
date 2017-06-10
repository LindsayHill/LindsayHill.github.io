---
author: lindsay
date: 2017-06-10
layout: post
slug: rpm-postupgrade
title: 'RPM Post-Upgrade Scripts'
categories:
- Open Source
tags:
- Linux
- RPM
---

Something different today: Here's something I learnt about RPM package management, and post-upgrade scripts. It turns out that they don't work the way I thought they did. Post-uninstall commands are called on both uninstall and upgrade. For my own reference as much as anyone's here some info about it, and how to deal with it.

## RPM Package Management

[RPM](https://en.wikipedia.org/wiki/RPM_Package_Manager) is a Linux package management system. It is a way of distributing and managing applications installed on Linux systems. Packages get distributed as .rpm files. These contain the application binaries, configuration files, and application metadata such as dependencies. They can also contain scripts to run pre- and post- installations, upgrades and removal. 

Using package management systems is a vast improvement over distributing source code, or requiring users to manually copy files around and run scripts themselves.

There is some effort required to get the spec files used to create packages. But once it has been set up, it is easy to create new versions of packages, and distribute them to users. System administrators can easily check which version they're running, check what new versions are available, and upgrade.

We use RPMs to distribute [StackStorm](https://stackstorm.com) packages for RHEL/CentOS systems. Similarly, we distribute debs for Ubuntu-based installations. These perform a similar function to RPMs, for Debian-based systems such Ubuntu.

## Customer Reports: Too Many Logs

We started getting reports from some users that they had too many StackStorm logs under `/var/log/st2`. This was surprising to us, since we include an [st2 logrotate configuration](https://github.com/StackStorm/st2/blob/master/conf/logrotate.conf) file in our standard packaging. This should exist at `/etc/logrotate.d/st2`. It contains rules for automatic compression and deletion of st2 logs. 

Yet some users said they didn't have this file. What was going on? We couldn't reproduce it. All our test systems had this file present.

But the file was missing for these users. `rpm -qV st2` told us that RPM expected it to be there, but it was missing.

The issue only came up occasionally. Most users weren't affected. Only a few people were. After a while I started to realise that they were all RHEL/CentOS users, and they had been using ST2 for a while. Maybe there's a common thread here?

## Smoking Gun: The `%postun` Step

During investigation, I came across this section of the RPM spec file:

```bash
%postun
  %service_postun st2actionrunner %{worker_name} st2api st2stream st2auth st2notifier
  %service_postun st2resultstracker st2rulesengine st2sensorcontainer st2garbagecollector
  # Wipe out st2 logrotate config, since there's no analog of apt-get purge avaialable
  [ ! -f /etc/logrotate.d/st2 ] || rm /etc/logrotate.d/st2
```

At first glance, it looks OK. `%postun` sounds like it should be run post-uninstall. It cleans up a leftover file, so it should be fine, right?. Problem is that this section also gets called at the end of an upgrade, not just on uninstall. That was the bit that caught me out.

From [Stack Overflow](https://stackoverflow.com/a/8072559):

> Yes, when an RPM upgrade occurs, RPM first installs the new version of the package and then uninstalls the old version of the package. Only the files of the old package are removed. But your scripts (i.e. `%pre`, `%post`, `%preun`, `%postun`) need to know whether they are handling an upgrade or just a plain install or uninstall.

## The Fix: Check this is Actually an Uninstall

This is the [PR I submitted](https://github.com/StackStorm/st2-packages/pull/419):

```diff
--- a/packages/st2/rpm/st2.spec
+++ b/packages/st2/rpm/st2.spec
@@ -60,8 +60,10 @@ Conflicts: st2common
 %postun
   %service_postun st2actionrunner %{worker_name} st2api st2stream st2auth st2notifier
   %service_postun st2resultstracker st2rulesengine st2sensorcontainer st2garbagecollector
-  # Wipe out st2 logrotate config, since there's no analog of apt-get purge avaialable
-  [ ! -f /etc/logrotate.d/st2 ] || rm /etc/logrotate.d/st2
+  # Remove st2 logrotate config, since there's no analog of apt-get purge available
+  if [ $1 -eq 0 ]; then
+    [ ! -f /etc/logrotate.d/st2 ] || rm /etc/logrotate.d/st2
+  fi

 %files
   %defattr(-,root,root,-)
```

When the `%postun` script gets called, it passes in "the number of packages that will be left after this step is completed." So this fix says "Only run this script if this is the very last instance of this package being removed or upgraded."

Now this step will only be executed on uninstall, not on upgrade.

## One Upgrade's Not Enough

The StackStorm packages have been updated, but unfortunately a single upgrade doesn't fully solve the problem. If existing users upgrade, it will run the current `%postun` scripts - so it will still delete the logrotate configuration file. 

Users have to carry out a second upgrade cycle. This time the config file won't get deleted, and all future upgrades will also be OK.

Annoying, but an interesting learning exercise.