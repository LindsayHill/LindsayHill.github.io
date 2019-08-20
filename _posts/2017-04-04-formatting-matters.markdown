---
author: lindsay
date: 2017-04-04 20:00:16+00:00
layout: post
slug: formatting-matters
title: Formatting Matters
categories:
- Opinion
tags:
- forums
- tools
---

Using proper formatting can make it much easier to read code and log samples. Yet so many people don't bother putting proper formatting around blocks of text. Take some time to learn how to format text in common applications and forums, and make things easier for those trying to help you.

## What's easier to read?

This?

> version: '2.0'
>
> examples.mistral-yaql-st2kv-user-scope:  
>     vars:  
>         polo: unspecified  
>     tasks:  
>         task1:  
>             action: std.noop  
>             publish:  
>                 polo: < st2kv('marco') %>  
>             on-complete:  
> \- fail: < $.polo != polo %>  

Or this?

```yaml
version: '2.0'

examples.mistral-yaql-st2kv-user-scope:
    vars:
        polo: unspecified
    tasks:
        task1:
            action: std.noop
            publish:
                polo: <% st2kv('marco') %>
            on-complete:
                - fail: <% $.polo != polo %>
```

Which one is easier to read? Which one lets you parse key information faster? Which one clearly shows file formatting and indentation? Obvious, right?

Yet far too often, I see people paste unformatted text into Slack, GitHub comments, and web forums. They dump huge blocks of unformatted, difficult to read code and logs. Even after repeated prompts to use proper formatting, they just dump big blocks of text.

The good thing is that it's not that hard to change the display formatting. Many applications contain shortcuts to make this easy. It's worth your time learning a few of the tips and tricks.

## Slack & GitHub

Both Slack and GitHub use a form of [Markdown](https://daringfireball.net/projects/markdown/syntax) to make it easy to quickly add formatting to text. The syntax varies slightly, but common things include:

* Put '\_' around text to _italicize_ it.
* Slack: Put asterisks '\*' around text to make it **bold**.
* GitHub: Put double-asterisks around text to make it **bold**.
* In-line fixed-width: Want to mention a command? Put backquotes around the command and it will be in `fixed-width`.
* Blocks of fixed-width output, e.g. from a shell session - Put 3 x backquotes at the start, and 3 at the end.

```bash
~ lhill$ snmpwalk 10.26.7.224 system
SNMPv2-MIB::sysDescr.0 = STRING: Brocade VDX Switch, BR-VDX6740, Network Operating System Software Version 7.1.0.
SNMPv2-MIB::sysObjectID.0 = OID: SNMPv2-SMI::enterprises.1588.3.3.1.131
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (522793200) 60 days, 12:12:12.00
SNMPv2-MIB::sysContact.0 = STRING: Field Support.
SNMPv2-MIB::sysName.0 = STRING: LEAF2
SNMPv2-MIB::sysLocation.0 = STRING: TME lab
SNMPv2-MIB::sysServices.0 = INTEGER: 79
~ lhill$
```

Those options work well for inline highlighting, or small blocks of fixed-width output. But when you've got larger blocks of code or logs, you should use [GitHub gists](https://gist.github.com/), or [Slack Snippets](https://get.slack.help/hc/en-us/articles/204145658-Create-a-snippet). These let you paste large blocks of code or logs, and choose the appropriate syntax highlighting. Slack will automatically collapse the text, rather than filling up everyone's screen. If you post your code/logs into a GitHub gist, then paste the link into a Slack window, it will auto-detect it as a Gist, and offer to import it for you. Neat! Wordpress has a [similar option](https://en.support.wordpress.com/gist/).

This code here is included just by adding a link to the Gist:

{% gist LindsayHill/ecfde3eb94c13672e026dafbeb6120ef %}

More reading

* Slack: [Format your messages](https://get.slack.help/hc/en-us/articles/202288908-Format-your-messages)
* GitHub: [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

## Why Does it Matter?

This is not just about making things look prettier. That's just a side-effect. By adding formatting, you're making it more readable. Make it more readable, and you're reducing cognitive load for other people reading the code, who are trying to help you. Make things easier for them, and they're more likely to help you.

It can help you too: Fixed-width formatting can highlight indentation or layout issues. Syntax highlighting can quickly identify simple errors with missing braces. There's a reason that many developers use editors that offer syntax highlighting.

## Do Us All a Favor

Learn some basic formatting tricks for Slack & GitHub, and whatever other forum you're posting content to. Reduce friction for those trying to help you. It's easy, and it makes a big difference. No excuses.
