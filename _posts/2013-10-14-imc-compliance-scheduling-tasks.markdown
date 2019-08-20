---
author: lindsay
date: 2013-10-14 14:00:30+00:00
layout: post
slug: imc-compliance-scheduling-tasks
title: IMC Compliance - Scheduling Tasks
categories:
- HP IMC
series: 'IMC Compliance'
tags:
- Compliance
- imc
---

{% include series.html %}

Compliance check tasks can be run immediately, or scheduled so that they run on a periodic basis - daily, weekly or monthly. This is extremely useful - by scheduling compliance checks, you can ensure that your systems maintain their security. It’s also very handy when you’re working your way through a huge list of items to clean up - scheduled tasks let you see progress, as the number of violations drops.

{% include note.html content="All screenshots below were taken using IMC 7.0. The screens will look quite different if you are still using 5.x. Most workflows are the same though." %}

To schedule a check, go to Compliance Center -> Check Task, and click Add:

[![Add Compliance Task](/assets/2013/09/compliance_add.jpg)](/assets/2013/09/compliance_add-e1380517204578.jpg)

Give your task a name, and delete the unwanted policies. From the “Execute Task” drop down, choose “Periodicially”:

[![Periodic Compliance Task](/assets/2013/09/compliance_periodic.jpg)](/assets/2013/09/compliance_periodic-e1380517329131.jpg)

Once you’ve done that, you will be presented options for Every Day, Every Week, or Every Month. Choose the appropriate schedule for you. When entering a time of day, you have to enter it as hh:mm:ss. Yes, that’s right - you need to specify the time down to the second when scheduling a policy compliance check. As if it matters.

Add your devices, or model types, and hit OK. It should now be scheduled - just one thing to check. Look at the “Status” column - make sure this shows “Enabled”. If the Status column shows disabled, the task won’t run periodically. Here’s what it looks like:

[![Enable Scheduled Task](/assets/2013/09/scheduled_task_enable-e1380517276809.jpg)](/assets/2013/09/scheduled_task_enable-e1380517276809.jpg)

In order to enable it, click on “Enabled” in the Operation column. Note that language here. You click “Enabled” to enable it, or “Disabled” to disable it. Something in the grammar got lost in translation from Chinese. Now you should be all set. You can either wait for the next scheduled run, or you can run it on an ad-hoc basis by selecting the task, and choosing Run.

If there are any policy check failures, IMC will create an alarm:

[![Compliance Alarm](/assets/2013/09/compliance_alarm-e1380517063538.jpg)](/assets/2013/09/compliance_alarm-e1380517063538.jpg)

{% include warning.html content="NOTE: This alarm is associated with the NMS itself, rather than individual non-compliant systems. So even if you’ve got 100 non-compliant systems, you’ll only get one alarm." %}

Re-running ad-hoc tasks is handy when you’re fixing systems, and need to re-run the checks to see if they now pass. Remember you’ll probably need to run a new backup before re-running the check.

The “Check Results” column will show the highest severity failures. Clicking on this severity will bring up the results, where you can drill into exactly what failed, and why.

## Scheduled Task Limitations

There are two key limitations to IMC scheduled tasks, and you must be aware of them. They both hinge around an inability to usefully modify a task once it has been created. **All you can change is the task scheduling, and enable/disable the task. Nothing else**.

If you have configured your task to run against a list of devices, you can’t change that list of devices. You can’t add or remove devices. If you add new systems to your network, you need to create a new scheduled task. This is at least semi-obvious, as you can see the list of devices in the task configuration. If your Check task runs against a list of device model types, it will pick up new systems.

The second, more subtle problem is that your scheduled task will take a point in time copy of the Compliance Policies and Rules. If you later tweak your rules, future runs of the scheduled task won’t take those changes into account. This is incredibly annoying - when you’re setting up your rules, you’ll probably create a few false positives. So you tweak your rules, and run the task again…only to find it still fails. After much wailing and gnashing of teeth, you figure out that it’s because IMC isn’t using your new rules. Delete the old task, create a new one, and presto!

## Better in Future?

It wouldn’t take much for HP to improve the IMC Compliance workflows, to remove those limitations. Changing to an alarm per failed device, and tidying up the Enabled/Disabled language would make it much more useful. Hopefully it’s something we’ll see sorted out sooner rather than later.
