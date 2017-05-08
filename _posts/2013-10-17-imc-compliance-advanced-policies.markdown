---
author: lindsay
date: 2013-10-17 15:00:15+00:00
layout: post
slug: imc-compliance-advanced-policies
title: IMC Compliance - Advanced Policies
categories:
- HP IMC
series: 'IMC Compliance'
tags:
- Compliance
- imc
---

{% include series.html %}

So far we’ve only looked at basic rules. Let’s take a look at some advanced uses of Compliance Center - advanced pattern matching with regular expressions, and Configuration Segment checks. The combination of these features lets us write far more advanced rules. You can solve almost any problem with these features, but be warned - it can take quite a while to figure out the exact syntax for your regular expressions.

> Some people, when confronted with a problem, think "I know, I'll use regular expressions."  Now they have two problems.

_From [Jamie Zawinski](https://groups.google.com/forum/?hl=en#!msg/alt.religion.emacs/DR057Srw5-c/Co-2L2BKn7UJ)_


## Advanced Pattern-Matching


When configuring a rule, in the ‘Match Mode’ section, the default ‘Rule Type’ is Basic. Let’s create a new rule, and change the Rule Type to Advanced:

[![Match Mode](/assets/2013/10/match_mode.png)](/assets/2013/10/match_mode.png)

Note that the options change, and now we have Operation, Match Mode, and Match Patterns. The key items here are Match Mode and Match Patterns. Match Mode can be either Included or Excluded. If we set our Match Mode to Included, then we are saying “Our configuration MUST contain this pattern. If it does not, then this check should fail.” If we choose Excluded, we are saying “Our configuration MUST NOT contain this pattern.”

The Match Pattern box is where we can enter our regular expression. This follows the rules of regular expressions. These are far more complicated than simple wildcards, and beyond the scope of this blog post. If you’re not familiar with these, I highly recommend reading one of the many tutorials available online. Here’s some examples of patterns you might use:


```bash
# Logging set to any loopback interface:
logging source-interface Loopback\S+
# BGP Neighbor authentication
neighbor [A-Za-z0-9]+ password [A-Za-z0-9]+
# Login banner defined:
banner \S+
```


Note that certain characters have special meaning - e.g. a `.` represents “any character.” So if you’re entering an IP address, you will need to escape the `.` with `\` - e.g. `10.1.10.10`.

You can add multiple patterns. Once the first is added, you will get a Rule Relation drop-down, which you can set to AND or OR. Using this, you can build up complex policies - e.g. “Configuration must contain Pattern A and Pattern B, but NOT Pattern C.” Be careful with just how complex you get. You'll probably need to work through it on paper first, to work out exactly what you need. Here’s a reasonably simple example of the sort of rule you could end up with:

[![Example Advanced Rule](/assets/2013/10/example_advanced_rule.png)](/assets/2013/10/example_advanced_rule.png)



## Walk-through Example


The best way to see how and why we would use Advanced Pattern-Matching is to use an example. Consider SNMP community string configuration on a Cisco device. If our chosen community string is “mySecretString”, then this is what our configuration should look like:


```text
sw02pi#sh run | inc community
snmp-server community mySecretString RO 99
sw02pi#
```


We could use a regular basic pattern match to look for that string, and it would correctly alert if that string was missing. But what if someone adds in an incorrect string? Cisco SNMP community configuration is additive. Look at this example, which starts with the earlier SNMP configuration:


```text
sw02pi#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
sw02pi(config)#snmp-server community public RO
sw02pi(config)#^Z
sw02pi#sh run | inc community
snmp-server community mySecretString RO 99
snmp-server community public RO
sw02pi#
```


Now I have a problem - how do I detect this sort of configuration? I could write a rule that alerts if it sees `snmp-server community public` - but what if there’s some other random string added? What I really need to do is find a way to say “Alarm if the device does not have my community string, and ONLY my community string”. Advanced Pattern-Matching lets us do this. Here’s how:

First add a new Compliance rule, set the vendor as required, and set the Rule Type to Advanced. First add a Match Pattern of `snmp-server community mySecretString RO \d+`. The Operation should be ‘Check’, and the Match Mode should be ‘Excluded’. If you’re not familiar with regex syntax, the `\d+` pattern says “match any digit, one or more times”. This will match any ACL number. So this first line will check that we have the right community string added, set to RO, and with an ACL defined. Now let’s add another line. This time, set the Match Mode to ‘Excluded’. Note we can also now set the Rule Relation - this should be AND. Set the Match Pattern as `snmp-server community (?!mySecretString)`. Add that. Your rule should now look like this:

[![Snmp String Rule](/assets/2013/10/snmp_string_rule.png)](/assets/2013/10/snmp_string_rule.png)

Hit OK to save the rule, and OK again to save the policy. Create a task to run the rule, and check the results. Here’s the output from running that check against my system:

[![Comm String Fail](/assets/2013/10/comm_string_fail.png)](/assets/2013/10/comm_string_fail.png)

Let’s fix up the device:


```text
sw02pi#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
sw02pi(config)#no snmp-ser comm public
sw02pi(config)#^Z
sw02pi#wr
Building configuration...
[OK]
sw02pi#
```


Run a backup in IMC, and re-run the Check Task. Success!

[![Comm String Pass](/assets/2013/10/comm_string_pass.png)](/assets/2013/10/comm_string_pass.png)


## Configuration Segments


Let’s say you want to check that your switchports have `no logging event link-status` added. If you just use a normal rule, and look for that pattern, how will you know that every interface has this configuration applied? The rule won’t count individual matches, it will see the first match, and then the device will pass. What you really want to do is check every single interface. But what about non-Ethernet ports? And what about trunk ports? Maybe I want to log link status changes on those. Bugger. Now it’s getting complicated.

Let’s see how we can write a rule that meets these requirements:


  * Check all GigabitEthernet ports, to see that they have `no logging event link-status` set.

  * This should only apply to access ports. Trunk ports should not have `no logging event link-status`li>
    
  * Non-Ethernet ports should not have `no logging event link-status` set.


Here’s an example of a typical interface configuration:


```text
interface GigabitEthernet0/3
 switchport access vlan 102
 switchport mode access
 no logging event link-status
 no snmp trap link-status
!
```


There’s two parts to this. First we need to define a rule that will look at interface configurations. Add a new rule, but this time change the ‘Check Type’ from the default of ‘Device’ to ‘Interface’. You’ll see that we now have a ‘Start Identifier’ and ‘End Identifier’ box. Our ‘Start Identifier’ will be `interface GigabitEthernet*` and our ‘End Identifier’ will be `!`. Cisco uses `!` between sections, while Comware uses `#`.

[![Interface Check](/assets/2013/10/interface_check.png)](/assets/2013/10/interface_check.png)

This setup will tell IMC to apply our rule only to the configuration under each GigabitEthernet interface. It will run this check against all GigabitEthernet interfaces on each device, and ensure they all comply. Now we need to define the actual pattern matching we need. We can’t just do a simple match for `no logging event link-status` because that will create a false positive on trunk ports. Thinking more about, we can see that an interface configuration needs to contain EITHER `no logging event link-status` OR `switchport mode trunk`.

Here’s one way of achieving that: First scroll down to the ‘Match Mode’ section, and change it to ‘Advanced’. Add `switchport mode trunk`, and click Add. Now set the ‘Rule Relation’ to OR, and add `no logging event link-status`. It should look like this:

[![Logging Event Status Rules](/assets/2013/10/logging_event_status_rules.png)](/assets/2013/10/logging_event_status_rules.png)

Because we’re using an OR match, only one of the conditions has to be true. When evaluating the condition, IMC will first look for the presence of ‘switchport mode trunk’ - if it sees that, it will stop evaluating, and pass that interface.

This rule can now be used in a policy, and deployed through a regular Check Task. When you run this task, if there are any non-compliant interfaces, you’ll see in the “Checked Content” link exactly which interface it was:

[![Check Content Interface](/assets/2013/10/check_content_interface.png)](/assets/2013/10/check_content_interface.png)

{% include note.html content="The sharp ones out there might say “What if we want to check that logging is ON for all trunk interfaces? This rule uses OR, not XOR, so a trunk could have logging disabled.” That’s true, and IMC doesn’t have an XOR Rule Relation. Instead, you’d need to build up a more complex rule that looked something like (`switchport mode trunk` AND NOT `no logging event link-status` OR (`switchport mode access` AND `no logging event link-status` There’s a few different ways of achieving it." %}


Other options for checking Configuration Segments are very similar, but can use different ‘Start Identifiers’ - this lets you check say EtherChannel interfaces, or sections like `router ospf` or `line vty *`.
