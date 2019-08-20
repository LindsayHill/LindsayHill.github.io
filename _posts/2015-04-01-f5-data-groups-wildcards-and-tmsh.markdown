---
author: lindsay
date: 2015-04-01 08:52:55+00:00
layout: post
slug: f5-data-groups-wildcards-and-tmsh
title: F5 Data Groups, Wildcards and tmsh
categories:
- Routing &amp; Switching
tags:
- F5
---

Just a quick note about a problem I ran into with adding data groups to an F5 system using tmsh. I wanted to add a string data group containing a list of URIs mapping to other URIs. This was for use in an iRule that will redirect these URIs.

So I thought that this tmsh script would do the trick:

```sh
modify ltm data-group redir_uris records add {"/first-uri" { data "/new-uri"}}
modify ltm data-group redir_uris records add {"/second-uri" { data "/new-uri"}}
```

Every time I tried it, I got this result:

```text
Syntax Error: the "create" command does not accept wildcard configuration identifiers
```

Hmm. But I don't have any wildcards. So what's the problem? I couldn't figure it out at the time, and ended up having to resort to manually entering the data group via the web interface. A bit slow, but luckily it was only around 20 entries.

Today I found out what was going wrong: [SOL12999: "Data group records beginning with a slash character cannot be added using tmsh."](https://support.f5.com/kb/en-us/solutions/public/12000/900/sol12999.html)

> **Description:** You cannot add data group records that begin with a slash ( / ) character to data groups using tmsh.
>
> This issue occurs when all of the following conditions are met:
>
> * You are creating or modifying a data group from tmsh.
> * The record you are adding to the data group contains a slash as the first character.

Workaround is to use the web interface, or bigpipe, e.g:

```text
bigpipe class MyClass \"/path/index.html\" add
```

It's fixed in 11.0.0. Just another reason to get off the 10.x branch.
