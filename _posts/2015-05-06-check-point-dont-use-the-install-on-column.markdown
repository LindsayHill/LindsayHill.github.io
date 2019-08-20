---
author: lindsay
date: 2015-05-06 09:58:16+00:00
layout: post
slug: check-point-dont-use-the-install-on-column
title: Check Point - Don't Use the 'Install On' Column
categories:
- Security
tags:
- Check Point
---

I got caught out by Check Point's "Install On" column recently. Most people don't need this setting any more, but it's still there for legacy reasons. Time to re-evaluate.

When you create a firewall policy using Check Point, you define the set of possible installation targets. That is, the firewalls that this policy may be installed on. When you compile & install policy, you can choose from this list of targets, and only this list.

{% include note.html content="In the 4.1 days, we didn't have this option. At install time, you had to choose from the complete list of firewalls. The default had all firewalls selected. You can imagine the merriment that ensued when someone would install the wrong policy on a firewall." %}

Most organisations will only have one installation target per policy. But sometimes you want to have the same policy on multiple firewalls. This is pretty easy to do, and might make sense if you have many common rules.

But then you say "What if I had 30 common rules, 50 that only applied to firewall A, and another 50 that only applied to firewall B?" That's where people start using the "Install On" column. This lets you define at a per-rule level, which firewall will enforce that rule. When you compile & install policy, it will only install the rules that apply to that specific firewall.

So in certain circumstances, it might make sense. But most organisations have one policy defined per firewall. In that case, you can just leave the "Install On" column at its default of "Policy Targets." There's no need to set it to anything else, since this policy is only ever going to be installed on one firewall.

But people see that column and think "Ah, I must have to define the enforcement point. I can see all these other old rules that have it defined." Everyone's too scared to change it, and so these things persist for 10+ years. Junior engineers keep needlessly setting it.

Every now and then it catches me out, when I copy a rule from one policy to another. The rule looks good - Source, Destination, Service are all set. I install the policy, no problems. No warnings, no errors. Check Point doesn't complain about a rule with "Install On" set to a firewall that's not even in the list of installation targets.

But then the damn firewall doesn't allow the traffic. The logs show drops, and I scratch my head for a while, trying to figure out why.

Eventually I cotton on to it, and reset it to "Policy Targets." Re-install policy, and we're away.

Then I go back to cursing the previous engineer for using the "Install On" option when there was no need to.

If you are still setting that option, ask yourself why. Are you installing the same policy across different firewalls, and you actively choose where each rule should be enforced? Or have you just not updated your practices since the 4.1 days? If you've only got one installation target, then just leave the "Install On" column at "Policy Targets."
