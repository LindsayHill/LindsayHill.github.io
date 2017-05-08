---
author: lindsay
date: 2014-02-26 05:43:55+00:00
layout: post
slug: ipv4-address-transfer-process
title: IPv4 Address Transfer Process
categories:
- Routing &amp; Switching
tags:
- APNIC
- IPv6
---

IPv4 exhaustion is a real issue for large parts of the world. IPv6 is the long-term solution, but it doesn’t solve today’s problems facing ISPs. Alternatives are needed - either CGN, or purchasing more IPv4 blocks on the open market. Recently I’ve been involved in the purchase & transfer process - here’s my notes on how it works within the APNIC region.

In spite of what [Slashdot thinks](http://tech.slashdot.org/story/14/02/17/1319204/whatever-happened-to-the-ipv4-address-crisis), we are indeed running out of IPv4 allocations, and fast. APNIC and RIPE have entered their final IPv4 allocation stages. Hopefully soon ARIN will also enter the IPv4 end-game, if only to speed world-wide IPv6 adoption. APNIC is only handing out one /22 to existing or new members. That doesn’t last long if you’re a growing ISP. Carrier-Grade NAT is one workaround, but it’s ugly, and has the potential for significant customer impact. This leads to high costs - some suggest $40/user.

An alternative is to acquire IPv4 address space from organisations that no longer require their historical allocations. Obviously this is not a long-term solution, but the costs can work out better than implementing CGN. There’s a few steps to go through to obtain and start using an address block like this. Here’s how it works in APNIC:




    
  1. Get pre-approval from APNIC.

    
  2. Find a willing seller.

    
  3. Agree a price and payment method

    
  4. Receive the address space into your APNIC account

    
  5. Update whois, BGP, etc

    
  6. Start using the new prefixes!



{% include note.html content="NB: IPv6 is the long-term solution, providing vast amounts of address space for anyone who needs it. It’s easy to sit in an ivory tower and declare that “ISPs should just implement IPv6. They should never implement something as ugly as CGN, or waste their time buying more v4 addresses - that’s old-school.” The problem with this attitude is that it completely misses the very real problems facing ISPs - they can’t simply pursue a v6-only strategy, as they would only be able to offer customers a partial Internet. The vast majority of content & other users need to be on v6 before ISPs can contemplate abandoning v4. Dual-stack is the only viable short-term strategy." %}




## Get pre-approval



APNIC must believe that you have a valid requirement for the extra address space. To speed things along, you can request [pre-approval](https://www.apnic.net/manage-ip/manage-resources/transfer-resources/pre-approvals/). This can be done before you have found/purchased address space, and is valid for 24 months. You will need to provide information on your current address usage, and expected growth rates. Get this step done as early as possible, as it may take a few days while they go back and forth, possibly seeking further information. I don’t know why they bother with this step - possibly to stop speculators accumulating large netblocks, for possible future profit?



## Find a willing seller



You need to find someone willing to transfer their netblock to you. There’s many different ways of doing this - you can deal directly with an organisation that you know wants to sell their prefixes, or you can go via a broker. Brokers act as middle-men. Sellers offer their blocks to the brokers, and the brokers find buyers. Brokers will have a list of current blocks and requested prices. Any prices quoted by the broker should be the price paid by the seller. Any commission is paid by the seller, not the buyer.

APNIC publishes a list of [brokers](https://www.apnic.net/manage-ip/manage-resources/transfer-resources/transfer-facilitators/), but there are also others. Brokers do make me somewhat nervous, and you might want to consider the location and reputation of any sellers or brokers - e.g. I ended up using a broker in the same region as me, run by someone I know by reputation. That makes some things a bit easier if there are any legal complications.

Note that you don’t HAVE to purchase addresses from the same region. It is possible to purchase an ARIN block, and get it transferred to APNIC. This will add a few days to the process, but it’s not material in the overall time-scale. I preferred to buy in-region, as it made things slightly simpler.

**UPDATE: **ARIN-based organisations may wish to use the "[Specified Transfer Listing Service](https://www.arin.net/resources/transfer_listing/index.html)" - this brings together potential buyers and sellers in the ARIN region.



## Price and Payment



Prices vary - in general, the larger the block, the lower the per-IP price. Large allocations (e.g. /16) might go sub-$10/IP, while smaller blocks (/20) will be $15+. ARIN prices seem to be a little lower at present, and there are a few more blocks available. No surprises - it reflects historical allocations vs current demand. Brokers will be able to advise you if the sellers are prepared to split a block - e.g split a /18 into two /19s. This will raise the per-IP price, as it incurs extra overhead by the seller.

You will agree on a price per-IP, and then sort out payment details. Many transfers are paid for using an “Escrow” service, because there is no pre-existing business-business relationship. The Escrow service acts as a trusted third-party. There is a fee payable for this - usually ~1% of the purchase price.



## Transfer Process



If paying via Escrow, the buyer pays the money to the Escrow service. When the Escrow service has received the funds, they advise the seller. The seller then transfers the prefixes to your APNIC account. You can then advise the Escrow service to release the funds to the seller.

APNIC will now check that you have a valid pre-approval, and advise you of the transfer fee. You must pay this transfer fee (based on the size of the block) before the prefixes are moved into your account. The addresses are now yours to use as you wish



## Updating Whois, etc



You will now need to update your APNIC details, to ensure that correct Whois and reverse DNS records are set up. Make sure that you’ve configured the reverse DNS zones on your DNS server.

Now update all the places you use those addresses - firewalls, border ACLs, address pools, BGP announcements, etc. You will need to make sure your upstream providers update their BGP prefix filters. This may take a while - get onto it as soon as possible! Use BGP Looking Glass services to verify that your prefixes are now being announced by your AS.

Geo-IP services need updating too. [Speedtest](http://www.speedtest.net) uses the [Maxmind](http://www.maxmind.com/) database. Submit updates [here](http://www.maxmind.com/en/correction). This may take a few weeks. Sometimes it's just a cosmetic issue, but it may cause real problems for customers that can't access services because the remote site has an out of date Geo-IP DB.



## Start using the addresses!



Run some tests, make sure that you’ve got connectivity, then start allocating addresses to customers! Now you’ve got some more head-room, and some more time up your sleeve, while we get IPv6 sorted out everywhere.
