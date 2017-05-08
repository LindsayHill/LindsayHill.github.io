---
author: lindsay
date: 2014-01-19 06:41:17+00:00
layout: post
slug: hp-sdn-app-store-might-work
title: The HP SDN APP Store - It Might Just Work
categories:
- Routing &amp; Switching
tags:
- hp
- SDN
---

HP has been laying out their SDN vision over the last few months. They want to develop a [complete SDN ecosystem](http://h17007.www1.hp.com/us/en/networking/solutions/technology/sdn/#tab=TAB1), including an Open Standards-based network that can integrate with common overlay models, development resources to help 3rd parties to develop applications, test & validation tools, right through to an "App Store" that will deliver applications to end-users.

One of the example applications they've been demonstrating is their UC&C Lync SDN application, and it's pretty cool:

<iframe width="560" height="315" src="https://www.youtube.com/embed/pXMUz3ULbP8?ecver=1" frameborder="0" allowfullscreen></iframe>

It's a great example of using new technical capabilities to solve real problems - and it's not data-centre focused like everyone else. But at HP Discover 2013 in Barcelona, Dave Larson ([HP Networking CTO](http://www.hp.com/hpinfo/newsroom/press_kits/2013/HPatVMworld2013/Dave_Larson_bio.pdf)) said (paraphrasing): “We spent two years working with Microsoft to develop that application. " That comment struck at the heart of something that’s been nagging at me about SDN:

> If it takes that much time and effort for two large companies to develop useful SDN apps, then what hope do the rest of us have? How can small to medium-sized organisations develop applications themselves?

I asked about this, and got these two answers (paraphrased):


  1. **Development resources didn’t exist:** It wasn’t just a matter of writing an application, HP/Microsoft had to also develop the development environment. SDKs, toolsets, libraries, etc didn't previously exist. Now that those have been established, HP + partners can develop new applications much more rapidly.

  2.  **Let someone else do the work:** Who says you have to develop applications yourself? Most software your business uses is written by someone else (either commercial or Open Source), so why not do the same for your network applications? HP’s SDN App Store will offer a marketplace where you can find, buy and download SDN apps, and be reasonably confident they will Just Work.


## That Might Just Work


This goes a long way towards addressing my key concerns. The market I look at is mostly small-medium Enterprises. For most of them, application development is unrealistic - so they need to be able to easily add externally-developed applications to their environment, to address specific issues they are facing. For those that do have internal development resources, it’s too much of a hurdle to do everything from scratch. They need the SDKs and related development resources to make it realistic.

HP is really hitting two key pain points, without forcing you into adopting a complete network architecture, ala VMware NSX, or Cisco ACI. Their approach instead lets you pick and choose which applications you want to add on top of a base programmable network. This offers a lot of flexibility, and can work well for organisations that want to slowly add new features to an existing network. It also offers something to mid-range customers, a market segment that I feel has been ignored in the rush towards hyper-scale Data Centre-focused solutions.


## Concerns


This doesn’t mean that I’m completely convinced yet. Areas of concern for me include:


  1. Will this lock me into HP equipment and software? Will I be able to take my applications, and move them to [Open Daylight](http://www.opendaylight.org/), instead of the [HP VAN SDN Controller?](http://h17007.www1.hp.com/us/en/networking/products/network-management/HP_VAN_SDN_Controller_Software/index.aspx) (This will probably depend on how the Northbound API standardisation works out).

  2. Support - How exactly will the support relationships work between HP and third-party developers? Will there be endless finger-pointing, or will the application validation processes be sufficiently rigorous that we get a minimum quality level, with clear demarcations to identify which module is at fault?

  3. Related to the above, what if HP makes it too difficult to get applications accepted, either for technical or political reasons? What if an application competes with an HP-developed app? Will they reject it? It does look like I can use a “private” Enterprise app store, so maybe that will allow side-loading.

  4. What if the App Store doesn’t get enough buy-in, and there’s only a few apps? What if I buy applications through the HP SDN App Store, but they don’t get enough traction, and the store gets shuttered? What happens then?


Some of these concerns may have already been addressed - HP’s published a lot of material, and I’m still digesting it.


## Will it Deliver?


Maybe. The vision feels like it is addressing many challenges and concerns. I don’t know if HP’s version will be the one that “wins”, but the overall approach makes it realistic for “normal” organisations to use SDN to solve real problems, without enormous investment. It’s going to be fascinating to see how it goes at launch later this year, and to watch the take-up.

{% include note.html content="Disclaimer: HP paid for my flights, accommodation & sundry expenses to attend HP Discover. They did not attempt to unduly influence what I write, nor did they compensate me." %}
