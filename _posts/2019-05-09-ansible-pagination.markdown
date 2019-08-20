---
author: lindsay
date: 2019-05-09 16:00:00+00:00
layout: post
slug: ansible-pagination
title: CLI Still Sucks for Automation
categories:
- Automation
tags:
- ansible
- networking
---

Using network CLI for automation has always been fragile. But it keeps surprising me with the way it breaks. This time, it was a combination of Ansible, Arista, `replace: config` and `terminal length` used as a **config** command.

{% include note.html content="I work for Extreme Networks. I would like to be clear that I am not in any way picking on Arista here. This is just one of those interesting combinations of interplay between various features and products. I have no doubt that Extreme products can be configured in a way that makes them break with Ansible too." %}

## The Problem

I often hang out in the [NTC Slack](https://slack.networktocode.com) channel. A user reported they were having a problem with Ansible and EOS. Basic changes worked, but when they used [eos_config](https://docs.ansible.com/ansible/latest/modules/eos_config_module.html) with the `replace: config` option, it just timed out. We knew basic authentication & connectivity was fine, it had to be something else.

But it made no sense, because these modules are widely used. What's going on?

## Background #1: Pagination

Some commands produce more than one screen's worth of output - for example, `show run` can be hundreds of lines long. Most screens don't have hundreds of lines, so pagination is used. The network device detects how many lines your terminal window has. When a command output is longer than that, it will pause at the end of each 'screen' length of output. Typically it displays something like `--More--`. Hit enter and the display will move up one line, hit space and it will move up a whole screen length.

This is all very useful if you're a human looking at the CLI output.

But as a machine, it's a pain. If you have a script interacting with the CLI, it has to detect the `--More--` prompt, and respond appropriately. Slows things down, is annoying.

That's why almost all network operating systems support some form of the [terminal length](https://www.cisco.com/c/m/en_us/techdoc/dc/reference/cli/nxos/commands/fund/terminal-length.html) command. This lets you over-ride the auto-detected terminal length. More importantly, if you enter `terminal length 0`, it will ignore any length, and just display all output in one long stream. That's the simplest for any CLI expect-style automation to deal with. Almost all automation tools will use this.

Normally this works well. When something like Ansible connects to a device, the first thing it does after login is run `terminal length 0`, and it no longer needs to worry about pagination. After that it just runs commands, and expects that the entire output will be returned, with no need to detect page breaks.

## Background #2: replace:config

The `eos_config` module supports the `replace: config` option. The default is to use `replace: line`, which only pushes changed lines if it detects a difference. That's what most people use. Occasionally you need the `replace: block` option, which will push all lines in that block, if any of them are different. By `block`, it means "this section of the code" - e.g. the interface configuration.

The `config` option is not documented. But it will push the entire config if any one line needs to be changed. I don't know exactly why you'd do this, but hey, it's an option.

## Combination == Problem. Why?

The user reported that they were unable to use Ansible with `replace:config`. Ansible reported a timeout - exactly the sort of thing I would expect it to do if it had not matched the prompt properly, or it was stuck at a `--More--` prompt.

But why? How could that be happening when Ansible modules are widely used with Arista? There's nothing unusual, no reason why it shouldn't work when many, many other people successfully use these.

Why would it be getting stuck with pagination when that gets disabled on login?

## What's Happening?

The key was this part of the [EOS docs](https://www.arista.com/en/um-eos/eos-section-3-9-command-line-interface-commands#ww1125116)

> The pagination setting is persistent if configured from Global Configuration mode. If configured from EXEC mode, the setting applies only to the current CLI session.

Hang on a second. On most devices I've used, `terminal length` is only used at the _EXEC_ mode. That is, it only applies to the _current login session_. It's not a global setting, it's an ad-hoc setting.

Arista EOS supports `terminal length` as a global **configuration** command. This system had `terminal length 20` in the configuration. On initial login, Ansible ran `terminal length 0` for that session, and all was well.

**BUT** When `replace: config` was used, and it detected a change, it re-entered the **entire** configuration, including the `terminal length 20` command. At that point, the system went back to enforcing a page length of 20. This confused Ansible, because it started seeing `--More--` prompts, and didn't know what to do with it.

## The Fix

It's a fairly simple fix here - remove the `terminal length 20` line from the configuration. In this case it was a command that had been added a long time ago, perhaps when automatic length detection didn't work. No longer needed, so remove the command, the terminal length stops getting messed up, and Ansible doesn't sit there stuck at a `--More--` prompt.

Now hopefully I'll remember this when I come across it in another 5 years...
