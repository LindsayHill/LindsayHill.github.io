---
author: lindsay
date: 2017-06-15
layout: post
slug: senior-devs
title: 'The Difference Between Proper Devs and Me'
categories:
- Open Source
tags:
- Coding
- GitHub
---

I spend a lot of time poking around with code, and I can figure out most integration challenges, and simple code fixes. But I do not call myself a developer. I know, we can argue about what constitutes a developer, but I don't really want to get into that. I'd just like to highlight something that showed the difference between the futzing about that I do, and the way a senior developer thinks about problems.

## StackStorm Documentation Process

We use [reStructuredText](http://docutils.sourceforge.net/rst.html) for [StackStorm documentation](https://docs.stackstorm.com). It's a form of markup language, with everything is written in plaintext. It gets parsed into HTML (and potentially other formats). The use of special punctuation marks and indentation tells the parser how to render the HTML - e.g. inserting links, highlighting text, bullet points, etc.

When I want to update our documentation, I create a branch on our GitHub [st2docs repo](https://github.com/StackStorm/st2docs). I make my changes, then create a Pull Request against the master branch. When I do this, it triggers our CircleCI checks. These checks include attempting to build the documentation, and failing if there are any parsing errors. If I've made a mistake in my syntax, it gets caught at this point, and I can't merge it until I've fixed the problems. Once I've fixed the issues, and my PR gets merged, we then have jobs that kick off building the documentation and pushing the content to our S3 buckets.

This usually works pretty well. Any mistakes I make get caught before we push things to production.

## Uh-oh…CircleCI Checks Failed

This week I created a [PR](https://github.com/StackStorm/st2docs/pull/521) to update our sensor documentation. CircleCI started its checks, and then…it [failed](https://circleci.com/gh/StackStorm/st2docs/85). Big red X. Hmm. I'm pretty good at RST syntax now, and I usually pick up those errors with my local checks before I push them to GitHub. What happened?

Here's the error message:

```sh
reading sources... [ 98%] webhooks
reading sources... [100%] workflows


Warning, treated as error:
st2/CHANGELOG.rst:113: ERROR: Unexpected indentation.

make: *** [.community-docs] Error 1

case $CIRCLE_NODE_INDEX in 0) make docs ;; 1) make bwcdocs ;; esac returned exit code 2
```

OK, so it's an RST syntax error. But I didn't change that file. What's going on?

If you dig into our documentation build process, it clones the main [StackStorm repo](https://github.com/StackStorm/st2) and uses the CHANGELOG.RST file from that repo to create docs.stackstorm.com/changelog.html.

When our developers make changes to StackStorm, they include an entry in that file to document the change we've made. The build process then prettifies that changelog and turns it into a more readable HTML file. 

Someone made a change to that file but had a small mistake in their syntax. When the file got pulled into the docs build process, it resulted in a failure.

## Fix? Or Not?

OK, no big deal, I've seen that a couple of times before. I'll submit a [PR to fix the syntax](https://github.com/StackStorm/st2/pull/3471):

```diff
-   #3335 #3463 [Nick Maludy]
+  #3335 #3463 [Nick Maludy]
```

Simple fix, remove the extra space, and should be fine, right?

Sort of. 

But that only fixes *this* instance of the problem.

## The "Proper" Fix

[Tomaz](https://github.com/Kami) submitted the **real** [fix](https://github.com/StackStorm/st2/pull/3472). His PR does this:

* Fix the indentation error
* Fix another problem with some duplicate content
* But more importantly: He adds linting checks that will get run on every PR to that repo in future, to ensure that any RST errors will be identified in that repo, pre-merge. 

That's the real difference. I wanted to just fix the immediate problem, and get on with what I wanted to do. Kami did the right thing, fixing the problem, and ensuring that if it happens again, it will be picked up earlier in the chain, to reduce downstream effects.

I really should know better. I know the pattern. But I still have a lot to learn, to make that sort of thing automatic when I'm working on code. 
