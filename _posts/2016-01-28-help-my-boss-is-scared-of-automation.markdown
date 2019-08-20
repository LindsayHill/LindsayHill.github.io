---
author: lindsay
date: 2016-01-28 05:19:37+00:00
layout: post
slug: help-my-boss-is-scared-of-automation
title: Help! My Boss is Scared of Automation!!!
categories:
- Worklife
tags:
- automation
---

A reader asked "What can I do if my boss won't let me automate my tasks?" Sadly some people still have a fear of automating even common, well-understood tasks. They're worried about automation run amok. They think it's safer to have a human typing in commands. But you know better. Humans have a place. But that place is not executing the same sequence of steps, over and over.

You need to prepare for change. Continuing to do repetitive tasks manually does not have a future. Either your boss will have a change of heart, or you're going to change jobs. You have to prepare yourself for either eventuality. Here's some thoughts on what to do.

## Just Do It

First option: Just do it. Don't bother asking, just get on with automating things you do often. You should be doing this anyway.

Last year we heard the story of a Russian hacker that had taken automation a little further [than usual](https://github.com/narkoz/hacker-scripts), with gems such as:

* **kumar-asshole.sh** - scans the inbox for emails from "Kumar" (a DBA at our clients). Looks for keywords like "help", "trouble", "sorry" etc. If keywords are found - the script SSHes into the clients server and rolls back the staging database to the latest backup. Then sends a reply "no worries mate, be careful next time".
* **hangover.sh** - another cron-job that is set to specific dates. Sends automated emails like "not feeling well/gonna work from home" etc. Adds a random "reason" from another predefined array of strings. Fires if there are no interactive sessions on the server at 8:45am.

Did he ask permission? No, of course not. No-one even knew those scripts existed until he left. But they automated common tasks, and made his life easier. You can do the same.

## Start with Low-risk/Low-fear items

Some people assume that automation means automating changes. But it doesn't have to just be for making changes. It can be for running reports, checking compliance, etc. A large percentage of an engineer's time is spent running 'investigation' commands, not making changes.

Look for good candidates for automation. That might be a 'bulk' task - e.g. "Find a list of all Windows VMs with VMware tools versions <= 9.4.0." But it might also be something you run often: "Find me all firewall rules for this destination IP." Create a script that takes an IP address as input, so you can re-run it as needed.

Start out with those, building up your experience with writing scripts, getting comfortable with the concepts.

## Script your changes (but manually run them)

Planning a middle of the night config change? Script all your steps, including your pre- and post-change checks. Don't worry about making an all-singing, all-dancing magical thing. It's fine to just prepare your commands in Notepad++, ready for pasting into the terminal. You're going to do _exactly_ the same thing as if you were doing it manually, it's just that your typing it out in advance. No problems with the boss.

When you go to make the change, paste in your pre-change checks. If those pass, paste in the changes, then run the post-change checks. If it fails, then paste in the 'undo' commands that you prepared in advance. Total change time: 5-10 minutes. It's bad enough being at work in the middle of the night, and all those numbers start to look alike when your eyes start getting blurry.

There's a huge difference between getting your work done in 10-15 minutes, and taking up the full two-hour change window. If it's a day-time change, and the boss thinks it will take you two hours, great! Now you can do something else more interesting during that time. Very helpful when you need a bit of creativity to fill in timesheets.

This scripting of your pre- and post-change checks will also stand you in good stead for when you move to full automation.

You can't always change other people's opinions. Gathering enough data & showing what can be done can help. But you can also prepare yourself for change, and you don't always need to ask permission. Decide what's right for you, your job, and your career.
