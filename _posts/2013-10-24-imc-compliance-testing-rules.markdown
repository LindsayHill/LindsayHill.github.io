---
author: lindsay
date: 2013-10-24 18:00:44+00:00
layout: post
slug: imc-compliance-testing-rules
title: IMC Compliance - Testing Rules
categories:
- HP IMC
series: 'IMC Compliance'
tags:
- Compliance
- imc
---

{% include series.html %}

It can be a slow process refining IMC Compliance policies. Configure a rule, create a task to run it, review the results, refine the rule, rinse, repeat. This is particularly tedious since you must define a new check task every time you update a rule - re-running a previously created task won’t work. IMC forces you to define a new task to make rule changes take effect. But there is something you can use to help: Testing.

Look at this list of rule in my “LKH-Custom Rules” policy. Note the “Test” Column:

[![Rules List](/assets/2013/10/rules_list.png)](/assets/2013/10/rules_list.png)

Click on the icon in the test column, and you’ll see a pop-up like this:

[![Test Popup](/assets/2013/10/test_popup.png)](/assets/2013/10/test_popup.png)

Doesn’t look particularly useful does it? But what we can do here is paste any scrap of config we like, hit “Test”, and IMC will tell us if the configuration would pass the compliance check or not. You can paste in as much or as little configuration as you like. You might paste an entire device config, or you might just paste in an interface configuration. My rule I am testing here is a simple one, that looks for `ntp server 172.16.2.40`. Let’s paste in some sample configuration, and hit Test:

[![Successful Test](/assets/2013/10/successful_test.png)](/assets/2013/10/successful_test.png)

You can see that this config passes, and that IMC has highlighted the key line. Let’s see what happens if our config didn’t pass. First click “Reset”, then paste in some different configuration:

[![Failed Test](/assets/2013/10/failed_test.png)](/assets/2013/10/failed_test.png)

This time we can it failed, as we expected. Our test is looking for an NTP server of 172.16.2.40, but this configuration uses an NTP server of 172.16.2.**50**.

You’ll also see an “Import” link there - this lets you import any previously defined configuration segment. These are configuration segments that you have defined for deployment to devices. What I would like to see here is a link to import any previously saved configuration backup - maybe we can hope for that in a future release? It would be a simple addition.

I recommend using this testing functionality, particularly when you’re trying to define complex regular expressions - it will save you time and effort.
