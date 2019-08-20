---
author: lindsay
date: 2013-10-17 16:00:59+00:00
layout: post
slug: imc-compliance-recovery-commands
title: IMC Compliance - Recovery Commands
categories:
- HP IMC
series: 'IMC Compliance'
tags:
- Compliance
- imc
---

{% include series.html %}

## What Are Recovery Commands?

Recovery Commands let us define the required configuration to fix any identified problems. When configuring a Compliance rule, you have the option to define Recovery Commands. If a system is non-compliant, you then have the option to automatically fix it, by running the defined Recovery Commands. This is great from a workflow perspective - define rules, identify non-compliant systems, then automate the fix. It’s not quite fully automated though - after identifying non-compliant systems, you need to tell IMC to go and apply the fixes. It also gives you a chance to review fixes before pushing them out.

## Walk-through Example

Let’s go through a fairly simple example. We want to check that our Cisco devices have an NTP server defined. If they don’t, the recovery command will be to add an NTP server. Straightforward, right? We first need to add a rule to one of our Compliance policies. Make sure the “Recover” radio button is set to Yes, and then we can add some recovery commands:

[![NTP Rule Recover](/assets/2013/10/ntp_rule_recover.jpg)](/assets/2013/10/ntp_rule_recover.jpg)

Now we can add a Check Task to run this policy against some test systems. I’ve got two Cisco switches here - one has an NTP server defined, and one doesn’t. Let’s run the check, and look at our results:

[![Task Result Fix](/assets/2013/10/task_result_fix.jpg)](/assets/2013/10/task_result_fix.jpg)

Note the “Fix” column has an icon displayed. Click on the “Fix” icon, and we’ll see a list of compliance breaches that we can fix. You can either select “Fix all devices”, or go through and select fixes on a per-device, per-policy or per-rule basis.

[![Fix Devices](/assets/2013/10/fix_devices.jpg)](/assets/2013/10/fix_devices.jpg)

Now click the “Fix” button. Note that we get one last chance to review the commands. We can even edit them at this point - this is particularly useful if you need to define variable names here. If you do define variables, using `${variable}`, IMC will make you replace it with a proper value before continuing. Very handy if you’re setting ACLs.

[![Fix Commands](/assets/2013/10/fix_commands.jpg)](/assets/2013/10/fix_commands.jpg)

Hit next, and you set the time to deploy the fix. The default is to deploy fixes one hour from now. This is quite clever, because it gives you a chance to not screw things up if you’re just clicking Next/Next/Next/Finish. I’m going to set mine to Immediate, and hit Finish:

[![Schedule Fix](/assets/2013/10/schedule_fix.jpg)](/assets/2013/10/schedule_fix.jpg)

You’ll now see the list of executing tasks. You’ll probably need to hit Refresh a few times, to see your task change from “Executing-Unfinished” to “Finished-Succeeded”:

[![Fix Task Result](/assets/2013/10/fix_task_result.jpg)](/assets/2013/10/fix_task_result.jpg)

If we drill into the task result, we can see the results per device. Clicking on the icon in the “View Change” column will even show you a standard Configuration Comparison window, highlighting the before and after differences in the configuration.

[![View Execution Result](/assets/2013/10/view_execution_result.jpg)](/assets/2013/10/view_execution_result.jpg)

## Conclusion

This was a somewhat contrived example. But imagine if we had to do this across 500 devices - that’s when this feature would really come into its own. For easily fixed configuration compliance issues, I highly recommend defining Recovery Commands.
