---
author: lindsay
date: 2018-10-26 14:31:54+00:00
layout: post
slug: more-extreme-ansible
title: More Ansible Modules for Extreme
categories:
- Automation
tags:
- Ansible
- Extreme
---

We published Ansible modules for [Extreme SLX devices]({% post_url 2018-03-09-ansible-slx %}) earlier this year. Now we have modules covering all the main Extreme Switching & Routing product families - SLX, VDX, MLX, EXOS, VSP.

## Available Modules

 * [SLX](https://docs.ansible.com/ansible/devel/modules/list_of_network_modules.htmli#slxos) - slxos\_command, slxos\_config, slxos\_facts, slxos\_interface, slxos\_l2\_interface, slxos\_l3\_interface, slxos\_linkagg, slxos\_lldp, slxos\_vlan
 * [VDX](https://docs.ansible.com/ansible/devel/modules/list_of_network_modules.htmli#nos) - nos\_command, nos\_config, nos\_facts
 * [EXOS](https://docs.ansible.com/ansible/devel/modules/list_of_network_modules.htmli#exos) - exos\_command, exos\_config, exos\_facts
 * [VOSS](https://docs.ansible.com/ansible/devel/modules/list_of_network_modules.htmli#voss) - voss\_command, voss\_config, voss\_facts
 * [MLX](https://docs.ansible.com/ansible/devel/modules/list_of_network_modules.htmli#ironware) - ironware\_command, ironware\_config, ironware\_facts

All modules are available in the current GA version of Ansible (2.7), except for `voss_config`. That one proved a bit trickier for me to write, and I didn't get it done in time for the 2.7 cutoff. That one is an open [Pull Request](https://github.com/ansible/ansible/pull/47533) against the Ansible `devel` branch. That should get reviewed and merged soon. It will then make its way into the next GA release. You can of course use the code direct from my branch in the meantime.

All modules use the `network_cli` plugin. See [Platform Options](https://docs.ansible.com/ansible/latest/network/user_guide/platform_index.html) for general information about how to use this connection type.

{% include warning.html content="These modules are not officially supported by Extreme. They are published as Open Source code, in the hope they will be useful to someone. Don't even think about logging a TAC case." %}

Thanks to my colleagues who helped with those modules.

## Future Ansible Work

I have no plans for writing additional Ansible modules right now. Writing roles that build upon [network-engine](https://github.com/ansible-network/network-engine/) makes more sense. 

That said, these modules will need some future work:

* Review all modules to ensure they are up to date with API changes, simmilar to the recent updates to EOS, IOS, Junos modules.
* Look at using `httpapi` connection plugin for devices that support it.
* Strip out the `connection: local` support from the Ironware modules. None of the other modules support this legacy method. This should be done in the 2.9 development cycle.

Questions/comments? Email me, or submit an issue. Better yet, submit a Pull Request.
