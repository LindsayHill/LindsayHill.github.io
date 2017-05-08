---
author: lindsay
date: 2015-03-28 02:52:27+00:00
layout: post
slug: hp-imc-silent-installation
title: HP IMC Silent Installation
categories:
- HP IMC
tags:
- Ansible
- hp
- imc
---

[HP IMC](https://www.hpe.com/networking/imc) installation is normally a manual process, with plenty of clickey clickey clickey. This is OK for production systems, as most sites will only have one or maybe two IMC servers. But for my lab, I wanted to automate the install, so I can quickly spin up a new lab system. I have now found an undocumented, unsupported way of doing this.

There's two parts to this - preparing the underlying OS & DB, and installing IMC. I am writing Ansible playbooks to handle the OS + DB setup. That's working, but it needs a bit of cleanup. Once that's done, I'll integrate it with Vagrant. Then I should be able to completely automate the install of a lab IMC system. I will write another post on that once it's complete.

To install IMC silently, create an `install.cfg` file to define your settings. Then tweak the installation script to call the silent installer, not the interactive install.

{% include note.html content="I am using CentOS 6.x plus MySQL 5.6. With a few tweaks, this will probably work with Windows and/or other DBs. Also remember that this does not seem to be publicly documented anywhere. I've figured out how to do it through a bit of trial & error, but I wouldn't go asking HP for support." %}




## install.cfg



The key file is `install.cfg`. Create this file in the same directory as the IMC install.sh` script. Here's what my `install.cfg` looks like:


```ini
install.dir=/opt/iMC
data.dir=/var/lib/mysql
db.type=MySQL
db.adminusername=root
db.adminpwd=iMC123
db.port=3306
install.locale=en_NZ
deploy.components=iMC-PLAT,iMC-EUPLAT,iMC-GAM,iMC-ACLM,iMC-DM,iMC-SEPLAT,iMC-REPORT,iMC-ICC,iMC-SCC,iMC-VNM,iMC-SYSLOG,iMC-NME-FAULT,iMC-NETASSET,iMC-VLAN,iMC-NME-PERF
http.port=80
https.port=443
```


Most of that should be pretty self-explanatory. Here's a breakdown of what they do, as far as I can figure out:




    
  * **install.dir** - the location where IMC is installed.

    
  * **data.dir** - location for the DB. This is more important for SQL Server installation.

    
  * **db.type** - choices are SQLServer, Oracle, MySQL, PostgreSQL. If you use Oracle, you also need to define "db.addinfo" (i.e. additional DB info). I don't know the exact syntax required for that, but I suspect it is the network service name.

    
  * **db.adminusername** - A user with SA-level privileges. This user needs to be able to do pretty much everything - create new databases, users, etc. Typically this will be root, or sa.

    
  * **db.adminpwd** - the password for the above user. Note that if you're using MySQL on the localhost, with user root, you must set a password for root. IMC won't let you have a blank password.

    
  * **db.port** - port the DB listens on.

    
  * **install.locale** - the [locale](https://en.wikipedia.org/wiki/Locale) in use. Unsurprisingly, I'm using English - New Zealand.

    
  * **deploy.components** - the IMC components to deploy. This is a comma or semi-colon separated list. This is the full list of components installed with IMC Standard. If you're installing Enterprise, there's a couple more you can add. If you find out what those other component codenames are, let me know.

    
  * **http.port** - port for IMC to listen on. Default is 8080, but I don't like that.

    
  * **https.port** - port for IMC to listen on for HTTPS. Default is 8443.



All pretty self-explanatory, right?

{% include note.html content="There's one small issue: you can't define the database server. From my analysis of the InstallSilence Java class, it will always use 127.0.0.1 for the DB IP. That's OK for a lab system, but in production you'd probably rather have an external DB. It's a pity you can't define it, because it wouldn't take much to extend the silent installer to handle it." %}




## install.sh



The default install script is `install.sh`. This needs a few changes for a silent install. This will tell the installer to call a different Java class, and to send its output to Xvfb. This way you don't need an X11 server running. I've created a new script `installsilence.sh` that contains these differences:


```bash
[root@imc ~]# diff install.sh installsilence.sh 
6c6
< MAIN_CLASS=com.h3c.imc.deploy.InstallLauncher
---
> MAIN_CLASS=com.h3c.imc.deploy.InstallSilence
32c32
< echo Loading install wizard ...
---
> #echo Loading install wizard ...
35c35,39
< "$JAVA_HOME/bin/java" -Xmx256m -Dinstaller.dir="$CURRENT_DIR" -DsupportedLocales="$INSTALL_LANG" -Dedition=STANDARD -Dcompany.flag=HP -D3CProductNumber=JG747AAE -DskipVmCheck=true -cp "$JAVA_CLASSPATH" "$MAIN_CLASS"
---
> # Set up Xvfb so that progress scrollbars get sent to "virtual" X11 server
> /usr/bin/Xvfb >/dev/null 2>&1 &
> export DISPLAY=:0
> 
> "$JAVA_HOME/bin/java" -Xmx256m -Dinstaller.dir="$CURRENT_DIR" -DsupportedLocales="$INSTALL_LANG" -Dedition=STANDARD -Dcompany.flag=HP -D3CProductNumber=JG747AAE -DskipVmCheck=true -cp "$JAVA_CLASSPATH" "$MAIN_CLASS" -y
[root@imc ~]#
```


The first change is to call the `InstallSilence` class, rather than `InstallLauncher`. This tells IMC to look for `install.cfg`, and read its settings from there.

Next I comment out an unnecessary `echo` statement. Then I run Xvfb, so that you don't need an X11 server set. This will send the progress bar off to the equivalent of `/dev/null`, and it won't block. You'll probably need to install `xorg-x11-server-Xvfb` first - I'm doing this in my Ansible playbook.

The last change is to add `-y` to the install command. If you don't do this, the installer will read the settings, tell you what it's about to do, then wait for you to enter `y` before continuing. With that setting, it won't prompt, but will just start the installation.

So far, this is all working well for me. I now have a completely repeatable process for installing IMC, and it should work with new versions too. Output during installation looks like this:


```bash
[root@imc ~]# ./installsilence.sh 
Loading install wizard ...
Install started at Sat Mar 28 10:18:30 NZDT 2015
Installation information:
    Install source dir: /root
    Install destination dir: /opt/iMC
    Locale: English (New Zealand)
    Components: [iMC-PLAT, iMC-EUPLAT, iMC-GAM, iMC-ACLM, iMC-DM, iMC-SEPLAT, iMC-REPORT, iMC-ICC, iMC-SCC, iMC-VNM, iMC-SYSLOG, iMC-NME-FAULT, iMC-NETASSET, iMC-VLAN, iMC-NME-PERF]
    Usable space: 9286 MB
--------------------------------------------------
Installing common components...
Installing component 'iMC-EUPLAT'...
Installing component 'iMC-GAM'...
Installing component 'iMC-ACLM'...
Installing component 'iMC-DM'...
Installing component 'iMC-SEPLAT'...
Installing component 'iMC-REPORT'...
Installing component 'iMC-ICC'...
Installing component 'iMC-PLAT'...
Installing component 'iMC-SCC'...
Installing component 'iMC-VNM'...
Installing component 'iMC-SYSLOG'...
Installing component 'iMC-NME-FAULT'...
Installing component 'iMC-NETASSET'...
Installing component 'iMC-VLAN'...
Installing component 'iMC-NME-PERF'...
All components installed
Deploying component 'iMC-PLAT'...
  Preparing for master...
(lots of output snipped here)
  Save new component deployed info.
  Generate Trial License.
  All works were done.
All components deployed successfully.
Press 'Enter' to launch DMA.
[root@imc ~]#
```


**[UPDATE 20150331] **You can see at the end that it says "Press 'Enter' to launch DMA." Normally it will block here. I created this Expect script to automatically handle this. Now it can be launched remotely, and run with no interaction at all:


```tcl

#!/usr/bin/expect

set timeout 900

spawn /root/installsilence.sh

expect {
 timeout { send_user "\nInstall script failed to complete before timeout\n"; exit 1 }
 "*launch DMA." { send "\04" }
}
```


Then all you need to do us run `installer.tcl`. Because I send "Ctrl+D", it doesn't even launch DMA. That means that everything is installed, but only the Deployment Monitoring Service is running. It would be nice to figure out how to tell IMC to start all processes via the CLI. I don't know any way to do that yet, although I'm sure it's possible.
