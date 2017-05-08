---
author: lindsay
date: 2013-04-27 03:25:44+00:00
layout: post
slug: imc-test-script-cleanup_parser
title: IMC - test script for Cleanup_Parser
categories:
- HP IMC
tags:
- imc
- perl
---

When developing IMC adapters, sometimes you have problems with parsing the output correctly. Adapters that use the CLI to retrieve a backup will have extra lines that creep in, such as the `show run` command. You may also have some lines you wish to strip out - e.g. you might want to remove the `ntp clock-period` line from Cisco devices, as it triggers false positives indicating a configuration has changed, when it really hasn't.

The parsing is handled by a Perl script - usually something like _RouterOS_Cleanup_Parser_Script.pl_. This perl script contains subroutines that get called by IMC, to do the parsing. Normally the subroutine will be named something like `cleanupConfiguration`.

When you're testing out your adapter, it can be tiresome to make a change, then run through the whole backup process again. What you really want to do is isolate the parsing function, so you can just focus on that, running lots of rapid tests, without having to wait for the rest of the backup process. Here's a quick and dirty way to do it:

First get some sample output you want to test with. One way is to login to the IMC server, cd to `C:\Program Files\iMC\server\bin`, and run `plink user@IP`. This will connect to the remote device. Enter credentials, and then run the `show run` (or equivalent) command. Capture the entire output of this session, and save it to a text file.

Next copy the _RouterOS_Cleanup_Parser_Script.pl_ file to a temporary location. At the end of this file, add this line: `1;` You must add this, or the next stage will get an error like `RouterOS_Cleanup_Parser_Script.pl did not return a true value at tester.pl line 7.`

Now create a little test Perl script called _imcParser.pl_ that looks like this:


```perl
#!/usr/bin/perl -w
use strict;

# Usage: perl imcParser.pl input_file
# Quick tester script for checking out parser scripts for IMC.
# Whatever parser is called here MUST end with "1;"
require 'RouterOS_Cleanup_Parser_Script.pl';

# These two lines remove the line terminator, so the input gets read into
# one big string, rather than an array
my $holdTerminator = $/;
undef $/;
my $config = <>;

my $cleanedConfig = cleanupConfiguration($config);

print $cleanedConfig;
```


The `require` line must specify the name of the Parser script you want to test. The rest of the script reads in your sample file from stdin, runs it through the parser subroutine, and prints the output. You can call it like this:


```shell
perl imcParser.pl mikrotik.txt
```


(where `miktrotik.txt` is the saved output from step 1).

This will print out the parsed output that IMC will store when running a backup. Now you can tweak your Perl script, and quickly test it, without needing to go through the whole IMC backup process. You'll also get better error reporting if you've got any syntax problems in your Perl script.

There's probably more elegant ways of doing this, but it gets the job done.
