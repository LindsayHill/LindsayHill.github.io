---
author: lindsay
date: 2018-09-29 18:01:54+00:00
layout: post
slug: ansible-dont-be-afraid-code
title: Ansible - Don't be Afraid of a Little Python
categories:
- Automation
tags:
- Ansible
- opinion
---

This year I've written several Ansible modules. It wasn't that hard, yet some people claimed they had been waiting "years" for those modules. There was nothing stopping anyone else doing it, yet they hadn't. There's a weird reticence amongst network engineers to learn or write any code, even when it could make a large difference to their job. People either do nothing, or they create complex Ansible playbooks to work around their reluctance to write Python. It's not that scary. Why don't people put in a bit of effort?

## Don't be Afraid of a Little Python

Ansible playbooks use YAML, a somewhat human-readable markup language. These are instructions for "what" Ansible should do - e.g. "Use the Cisco [ios_config](https://docs.ansible.com/ansible/2.5/modules/ios_config_module.html) module to ensure that this configuration line <x> is present."

The underlying modules use Python. These are the "how" - they take the instructions from the playbooks, and turn those into device connections to devices, making configuration changes, checking state, etc. 

Some people look at these modules as a mystery black box that only the vendor can write. They think that the only way they can interact with Ansible is via playbooks.

This leads to two situations:

1/ Twiddling thumbs for years, continuing with manual operations, while waiting for someone else to write a missing module.

2/ Trying to use YAML as a programming language, leading to this:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Writing YAML <a href="https://t.co/NPA6P3GJg0">pic.twitter.com/NPA6P3GJg0</a></p>&mdash; Justin Palmer (@Caged) <a href="https://twitter.com/Caged/status/1039937162769096704?ref_src=twsrc%5Etfw">September 12, 2018</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

Both of these situations are a mess. They are unnecessary if people would get over their fear of writing a little code.

## Ansible Modules: Not That Scary

This year I decided to write some Ansible modules for various Extreme Networks products - SLX, VDX, VSP. I also reviewed the MLX modules, and contributed a little to some EXOS modules. 

I am not a full-time developer, nor do I play one on TV. But you know what? It wasn't that hard. I found some other molds that worked in a similar way (IOS), and spent a bit of time reading those, then copied & tweaked them to make them work for SLX/VDX/etc. The code is in Python, and it isn't all that complicated to re-work some existing modules. 

I have seen posts from people saying that they have > 1,000 Extreme devices, and **needed** Ansible modules to manage them. Those posts were from years ago, yet they never wrote modules themselves. They sat and waited for the vendor to write them.

I can understand this if it was something genuinely new, with no prior art to work from.

But that's not true at all here. There's plenty of code to build from, easy access to documentation and development environments, and no shortage of forums for help. Yet no-one else bothered to write those modules. 

Makes you wonder: How serious were people when they said they "NEED" these modules? No-one cared enough to write them. It didn't take a huge investment in time. So either it wasn't a priority, or people were too scared of writing a little Python.

## YAML 

Like Jeff Geerling [says](https://www.ansible.com/blog/make-your-ansible-playbooks-flexible-maintainable-and-scalable):

> YAML is not a programming language.

Quite so. Just because you _can_ do something with a combination of [YAML](http://yaml.org) & [Jinja2](http://jinja.pocoo.org), doesn't always mean you should. To quote a little more from Jeff:

> As a rule of thumb, if you've spent more than 10 minutes wrestling with escaping quotes in a when condition in your playbook, it's probably time to consider writing a separate module to perform the logic you need to do for the task. 

If you need to do complex data manipulation, you'll come unstuck if you stay within the confines of YAML. Write a Python module instead. Then you have a full set of tools to do whatever complex conditional or manipulation you need. 

## Conclusion

Don't be afraid of writing a little Python. If you have a real automation problem to solve, then invest a little time into it, and make a start on writing some code. Take charge of your own environment. Don't sit back waiting for a vendor to solve all your problems. You'll make much greater progress if you do.
