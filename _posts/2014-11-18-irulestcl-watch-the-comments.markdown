---
author: lindsay
date: 2014-11-18 23:07:16+00:00
layout: post
slug: irulestcl-watch-the-comments
title: iRules/Tcl - Watch the Comments
categories:
- Routing &amp; Switching
tags:
- F5
- programming
- TCL
---

It's pretty common practice to 'comment out' lines in scripts. The code stays in place, but doesn't get executed. Perfect for testing, when you might need more debug output, or you want to run a slightly different set of actions. But you have to be careful when commenting out lines - it might catch you out, and the F5 iRules editor won't save you.

Normally it's pretty simple to comment out a line. Here's a quick Bash example:

```bash
#!/bin/bash

FILECOUNT=`ls /tmp|wc -l`

if [ $FILECOUNT -lt 7 ]
 then
        echo "There are fewer than 7 files in /tmp"
        run_command
fi
...
```

When I'm testing the script, I might not want to actually run that command. So I'll quickly comment it out like this:

```bash
#!/bin/bash

FILECOUNT=`ls /tmp|wc -l`

if [ $FILECOUNT -lt 7 ]
 then
        echo "There are fewer than 7 files in /tmp"
        #run_command
fi
...
```

The `#` tells the shell to ignore anything else on that line. All pretty straightforward.

Today I was debugging an F5 synchronisation issue, where I got this message on synchronisation:

```text
BIGpipe parsing error (/config/bigip.conf Line 333):
   012e0054:3: The braced list of attributes is not closed for 'rule'.
```

The offending section looked like this:

```tcl
when HTTP_REQUEST {
  switch -glob [string tolower [HTTP::host]] {
    "lkhill.com" {
      switch -glob [string tolower [HTTP::uri]] {
        "/special/uri/path/*" {
          pool pool-special-application
          #log local0. "Re-mapped special URI" }
        default {
          pool pool-lkhill-default
          #log local0. "Default pool used" }
      }
    }
  }
}
```

Editing this iRule through the GUI didn't have any errors, so what was broken?

Looking at the 'log' lines a bit more closely, I saw the problem:

```tcl
        "/special/uri/path/*" {
          pool pool-special-application
          #log local0. "Re-mapped special URI" }
        default {
          pool pool-lkhill-default
          #log local0. "Default pool used" }
```

By commenting out the log line, it included commenting out the closing '}'. Whoops. Fixed now. How come it didn't get picked up in the iRule Editor? Looks like it's this bug: [SOL15363](https://support.f5.com/kb/en-us/solutions/public/15000/300/sol15363.html) "An iRule may erroneously pass syntax validation"

Oh and of course I know someone out there is saying "Why the hell did he use that stupid formatting for his code? My preferred style would be to have the closing brace vertically aligned with the opening statement, like this:

```tcl
when HTTP_REQUEST {
  switch -glob [string tolower [HTTP::host]] {
    "lkhill.com" {
      switch -glob [string tolower [HTTP::uri]] {
        "/special/uri/path/*" {
          pool pool-special-application
          #log local0. "Re-mapped special URI"
        }
        default {
          pool pool-lkhill-default
          #log local0. "Default pool used"
        }
      }
    }
  }
}
```

But it wasn't my code - I was fixing someone else's code.
