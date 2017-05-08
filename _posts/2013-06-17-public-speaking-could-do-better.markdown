---
author: lindsay
date: 2013-06-17 03:26:09+00:00
excerpt: I spoke at HP Discover in Las Vegas this year, on IMC Customisation, with
  Chris Young, Aaron Paxson, and Rick Kauffman. Overall I felt it was OK, but we could
  have done better. Here's a few of the lessons learnt.
layout: post
slug: public-speaking-could-do-better
title: Public Speaking - Could Do Better
categories:
- Worklife
tags:
- conference
- imc
---

I spoke at [HP Discover](https://www.hpe.com/events/discover/) in Las Vegas this year, on IMC Customisation, with Chris Young, Aaron Paxson, and Rick Kauffman. Overall I felt it was OK, but we could have done better. Here's a few of the lessons learnt.

Our talk covered three areas - adding device adapters to IMC for configuration management, adding custom functions, and using the eAPI. I handled the first part, Aaron spoke about custom functions, and Chris and Rick spoke about the eAPI.  One of the interesting challenges was in organisation - we all live in different parts of the world, making scheduling a problem. Dividing the talk into three clearly defined areas helped get around some of those problems.

The intention was to cover the technical details behind the configurations, then do a live walk-through of the real configuration. By using real-world examples, we hoped to show that it is straightforward to extend IMC, but there are a few gotchas. We wanted to be able to pass on some of the things we've discovered about IMC, to make it easier for others in future.

The session was only a small one - around 18 places - but it was full. It was a little surprising to see quite that much interest in a niche topic. Later we realised that this was partly because some people who were at the session didn't realise just how niche it would be. It wasn't a session for learning about IMC, but perhaps we didn't make that clear enough in the session abstract.

There's a few areas I'd like to improve on, should we do it again.


## Lessons Learned





	
  * **Know your audience - and make sure they know you**: Our session was a deep(ish) technical session. It was not an introduction to the product. It was for those already using it. But we had some people there who had never seen IMC before. At the start of the session, I should have spent more time gauging participant knowledge levels - and giving people a chance to leave early!

	
  * **Minimise the chance of demo gremlins**: We had a local demo environment, with a laptop and several network devices. Things had checked out the previous night. But shortly before kick-off, we had some systems refusing to start. Fall-back systems had the wrong versions of software, and we also ran into issues with display adapters. As I was starting the session, Chris was still trying to get things running. This meant a bit of on-the-fly reshuffling of content. Hopefully I covered it well enough. In future I'd rather have a semi-permanent remote demo lab up and running, ready to go. Then all that is required is decent Internet access - oh and of course a video to fall back on (Yes Chris, you were right about that).

	
  * **Ensure the content flows well**: The concepts and code for configuring device adapters and custom functions are closely related. We didn't do enough to stress these similarities, and to minimise overlap between different parts of the presentation. This probably came about because of the way we each developed our own sections.

	
  * **Keep the audience engaged**: Two hours of looking at XML and TCL files can get a bit tough, both for presenters and attendees. We needed to figure out a way to get participants doing some hands-on tasks, so they keep engaged. It's also easier for people to look at code and logfiles on a screen in front of them, rather than in tiny letters on a big screen.



Overall I'm happy with the session - it could have been a complete disaster, but it wasn't too bad, given the demo challenges. The key problem to address is keeping people  engaged over a longer session. Hopefully we get a chance to run the session again, and make it much tighter.

You can read the session presentation [here (warning PDF)](/assets/2013/06/HOL2809.pdf). Any further questions, feel free to [get in touch](/contact/).

{% include note.html content="Disclaimer: HP paid for my flights to HP Discover in Las Vegas, my accommodation, and for some meals. They did not pay me, nor did they specifically ask me to write anything. " %}

