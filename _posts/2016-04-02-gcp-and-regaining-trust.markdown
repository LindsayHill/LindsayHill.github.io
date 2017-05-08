---
author: lindsay
date: 2016-04-02 05:03:18+00:00
layout: post
slug: gcp-and-regaining-trust
title: GCP, and Regaining Trust
categories:
- Opinion
tags:
- opinion
- rant
---

Google is telling us they’re serious about the [cloud](https://cloud.google.com). They’re hiring the [right people](http://fortune.com/2015/11/19/google-diane-greene-cloud/), spending the big bucks, and even (gasp!) _talking to customers_! (Oh how that must stick in their craw). They have great technology, they’ve proved it out at scale, and the price is right.



There’s just one nagging doubt in the back of our minds. Is Google serious about this? Are they going to turn around one day and say “GCP is too hard to maintain, we’re dropping it. Besides, self-driving Segways are the future.”



## Fool me once…



Because they have form in this. I present Exhibit A, [Google Reader](http://www.google.com/reader/about/). Yes, that old saw. Yes, yes I am still bitter. No, I won’t let it go.

I used Google Reader daily. I loved it. It came from a pre-Twitter, pre-Facebook time. A time when we used to have to visit a list of sites to keep up with things. We’d have to remember to check our friend’s travel blog every few weeks, just in case there was a new post. Sure, we used [Slashdot](http://slashdot.org) as an aggregator, but everyone knows that’s been dead/dying since Rob Malda sold out to the man. (Has Netcraft has confirmed it?)

Google Reader solved that problem for me, and I loved it.

And Google killed it.

Apparently it was ‘too hard to maintain’ - try telling that to the very small companies that set up similar services after Google killed it. 

So now Google’s telling us that they’re totally committed to the cloud, and we should sign up because they have the best technology. That [may well be true](https://quizlet.com/blog/whats-the-best-cloud-probably-gcp), but here’s the thing about Reader: Technologists loved it. And guess who decides which public cloud to build services on? Technologists. And we don’t _quite_ trust Google. 

This isn’t just my opinion. Check out the comments on this [HN thread](https://news.ycombinator.com/item?id=11159840)

> Google seems to be facing a trust problem here that its competitors don't, due to its history of shutting down popular services, even those that were being used internally and even those that it had been recently hyping to the press...
> 
> That's why I wouldn't build a business on Google, because they have a long history of killing things when they aren't wildly profitable/successful. A $10m/yr profit product is considered a distraction of valuable engineer time unless it has some ulterior goal for the company...
> 
> The problem (I suffer from it as well), is that people naturally feel that the corporate offerings from Google will mirror the experience that they have with the consumer offerings from Google. And the big issue with Google's consumer offerings is that they seem to have limited to no support and that there is a very real chance that Google will just shut them down all of a sudden...

How many people will spend their cloud dollars on AWS or Azure over GCP because of this? I have no way of measuring it, but I’m sure it’s non-zero. All for a service that Google could have easily maintained, but they killed because they weren’t selling enough ads on it.

You broke our trust Google, and it takes a long time to forgive.



## What about Amazon?



So far, Amazon has not given the impression that they will drop services they’re no longer interested in. They seem to be going the other way, adding services as fast as they can. Right now they’re more likely to replicate the functionality of popular services built on top of AWS, and make it a base AWS service. (Don’t act surprised by this, it’s a key part of their strategy. Read up on ILC for more).

I’m sure at some point Amazon will want to retire services. If Aurora proves its worth over the long run, and provides a superset of RDS services, why keep them both running? But I get the feeling they’ll do that based on demand, and they’ll just progressively shift the pricing mix until customers move on their own.



## How do they regain trust?



We still don’t quite trust Google to not say “Nah, we’ve lost interest in GCP. We’re building a space elevator.” Even if GCP was a $5 billion dollar business, it would be dominated by their advertising revenue. 

How will Google regain trust? There’s not much they can do but persevere, and build business the long, slow, hard way. If they can get to the point where GCP is even a third of advertising revenues, then maybe, just maybe I’ll believe they’re serious about it.

**UPDATE:** I wrote this just before Google announced [Yet Another Product Decommissioning](https://medium.com/@arlogilbert/the-time-that-tony-fadell-sold-me-a-container-of-hummus-cb0941c762c1#.a4ploimrg)
