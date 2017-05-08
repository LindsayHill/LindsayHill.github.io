---
author: lindsay
date: 2015-04-08 21:00:26+00:00
layout: post
slug: reminder-solarized-for-better-terminals
title: 'Reminder: Solarized for Better Terminals'
categories:
- Worklife
tags:
- putty
---

I have used the "[Solarized](http://ethanschoonover.com/solarized)" colour scheme on my Mac for several years. This is:


> ... a sixteen color palette...designed for use with terminal and gui applications


If you spend a lot of time using the Terminal, this makes a huge difference. It gives me the right combination of colours to make sure everything is readable, and reduces eye-strain.

I've used it for so long that I've forgotten about it. It's become "normal" for me.


## PuTTY Defaults == Unusable



Recently I've been forced to use PuTTY on Windows. I'd forgotten how terrible the default colour scheme is, particularly when you're using VIM, or doing an `ls` on a RHEL system. Check this screenshot:

[![putty_default](/assets/2015/04/putty_default.png)](/assets/2015/04/putty_default.png)

The default `LS_COLORS` on a RHEL system, using PuTTY defaults, will displays directories in dark blue on a black background. Hopeless. I can't read those directory names.



## Solarized to the rescue



I downloaded the "Solarized Dark" registry file from [here](https://github.com/brantb/solarized/blob/master/putty-colors-solarized/solarized_dark.reg). Double-click that to merge the registry settings. You'll then see a new PuTTY Saved session "Solarized Dark":

[![putty_sessions](/assets/2015/04/putty_sessions.png)](/assets/2015/04/putty_sessions.png)

Load that session. Save it as the Default Settings if you like. Add any other settings you need - e.g. username, SSH key. Add the hostname/IP, and connect. Now see how that `ls` command looks:

[![putty_solarized](/assets/2015/04/putty_solarized.png)](/assets/2015/04/putty_solarized.png)

Much better. Now I can actually read it. This also applies to syntax highlighting using VIM, etc.

Highly recommended. I suggest you also try Solarized, for better keyboarding.
