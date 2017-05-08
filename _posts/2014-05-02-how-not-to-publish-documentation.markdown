---
author: lindsay
date: 2014-05-02 08:38:19+00:00
layout: post
slug: how-not-to-publish-documentation
title: How Not to Publish Documentation
categories:
- Opinion
tags:
- cisco
- hp
---

Good documentation is critical to the success of any product. Write clear deployment & configuration information, and you'll have a higher project success rate. Detailed command references and troubleshooting information shorten the time it takes to resolve issues, and reduces support calls. But writing high-quality content is only one part of it. You have to make sure that the documentation is easy to find, and easy to access.

Recently I've been reading some [HP Networking](http://www.hp.com/go/networking) documentation, and it's been a study in how NOT to publish docs. Let's take an example - I want to see the Release Notes for the latest firmware for the 12504 switch. Here's the workflow:

First go to [HP.com](http://www.hp.com/). There's a drop-down "Support" - that looks likely. Click on that, then "Support & troubleshooting":

![support_page](/assets/2014/05/support_page.png)

Enter "12504" in the Product name/number box, and hit search. We get 3 results:

![3 matches](/assets/2014/05/3-matches.png)

Looks good. Let's click on "[HP 12504 AC Switch Chassis](http://h20180.www2.hp.com/apps/Nav?h_pagetype=s-001&h_lang=en&h_cc=us&h_client=S-A-R163-1&h_page=hpcom&lang=en&cc=us&h_product=5304955)". Hmmm. A very jarring change of layout, but it looks like we're getting closer:

![hp 12504 Support Page](/assets/2014/05/hp-12504-Support-Page.png)

Let's follow the link for "[Manuals](http://h20566.www2.hp.com/portal/site/hpsc/public/psi/manualsResults/?sp4ts.oid=4177453)":

![12504 Manuals](/assets/2014/05/12504-Manuals.png)

Software Reference sounds likely, let's take a look at that:

![software reference](/assets/2014/05/software-reference.png)

The version information seems to be mixed in with the Title there, but it looks like the most recent file should be the one. Except..it says it's a PDF. Why not an option to view in HTML or PDF? HTML is much better suited to online reading, but I can understand that some people prefer a PDF for offline viewing. Note the URL: [http://h20565.www2.hp.com/portal/site/hpsc/template.BINARYPORTLET/public/kb/docDisplay/resource.process/?spf_p.tpst=kbDocDisplay_ws_BI&spf_p.rid_kbDocDisplay=docDisplayResURL&javax.portlet.begCacheTok=com.vignette.cachetoken&spf_p.rst_kbDocDisplay=wsrp-resourceState%3DdocId%253Demr_na-c04220329-1%257CdocLocale%253Den_US&javax.portlet.endCacheTok=com.vignette.cachetoken](http://h20565.www2.hp.com/portal/site/hpsc/template.BINARYPORTLET/public/kb/docDisplay/resource.process/?spf_p.tpst=kbDocDisplay_ws_BI&spf_p.rid_kbDocDisplay=docDisplayResURL&javax.portlet.begCacheTok=com.vignette.cachetoken&spf_p.rst_kbDocDisplay=wsrp-resourceState%3DdocId%253Demr_na-c04220329-1%257CdocLocale%253Den_US&javax.portlet.endCacheTok=com.vignette.cachetoken)

Yes, seriously, that's the URL. Let's see what happens when we try to load it:

![adobe error](/assets/2014/05/adobe-error.png)

Really? REALLY??? OK, so maybe it's something to do with the PDF rendering in my browser (Chrome in this case, but Safari/Preview sees the same thing). Let's try doing a right-click, Save Link As on that URL:

![Download Title](/assets/2014/05/Download-Title.png)

I kid you not. **Every. Single. Document**. that you want to download from HP Networking wants to save as "Download.pdf." Unless maybe it's a zip file...and then it's "Download.zip." So now I need to manually re-name everything, and of course I can't just do a rapid right-click, download on a whole lot of links. Instead I need to pay attention, manually re-name, and then I end up with a heap of inconsistently named files.

Does anyone think about how real engineers are supposed to use this stuff? It really doesn't need to be this hard. It is 2014. There is no excuse for making documentation difficult to use. HP Networking should be embarrassed by this.

[UPDATE 15/7/14] HP now has a KB article on "[Resolving PDF Portfolio Viewing Issues](http://h20565.www2.hp.com/portal/site/hpsc/template.PAGE/public/kb/docDisplay/?spf_p.tpst=kbDocDisplay&spf_p.prp_kbDocDisplay=wsrp-navigationalState%3DdocId%253Demr_na-c04054430-2%257CdocLocale%253D%257CcalledBy%253D&javax.portlet.begCacheTok=com.vignette.cachetoken&javax.portlet.endCacheTok=com.vignette.cachetoken).' Among the other notes, it says that Portfolios may not open because Adobe Reader no longer installs Flash by default. If a PDF viewer requires Flash, that should be a big red flag telling you to run away now. Maybe they'll move to a version that also requires Java?
