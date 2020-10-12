---
layout: post
title:  "Introducing Conway's Law"
author: alberto
date:   2020-06-16 10:20:32 -0400
categories: transformation
tags:
- engineering culture
- microservices
- lead time
excerpt: Digital transformation is not just about the tech. It's about people, process and tools (in that order). As many industries continue their transformation journey, some core principles are arising. In 1967, Melvin Conway introduced a concept that is now known as "Conway's Law". It is very commonly used to describe how systems are designed and how teams are limited by communication factors.  In this post, we introduce these.
---
Software development is both an "art" and a "science". One has to recognize when both are needed. Too much "art", and you miss the "engineering" piece of it. Too much science, and you lose the intrinsic elements of the experience such as "culture" and "outcome focus". In fact, the hype about "Digital Transformation" is all about finding a balance between "art and science". Today, I'll present one of the "science" elements explained in Conway's law:

> "Any organization that designs a system will inevitably produce a design whose structure is a copy of the organization's communication structure." [^1]

![Conway's Law](/assets/img/2020/conway-law-view.png#imageInPost "Conway's Law")

## It's a People, Process, and Tools Problem
 It's interesting to think that this is not a new problem. The industry has been using _Distributed Systems_ for a very long time and organizations have been verifying the effects of Conway's Law along with its use. It's important to recognize that _Micro-Services_ architectures are a type of _Distributed Systems_, and organizations adopting it's use (whether because of hype or need) can benefit from the lessons of the past (in this case, Conway's Law)

 It follows then that the results of Conway's Law will be visible in not just our system design, but also the organization's culture, and process. Furthermore, Conway's effect suggests the need of a organization structure change at the team level coupled with a process that focuses on a value stream. In other words, the science now leads us to the "art" of communications in _high performing teams_.

 ![Communication Patterns](/assets/img/2020/communication-patterns-for-high-performing-teams.jpg#imageInPost "Communication Patterns")

## Tackling the Culture
Building complex system is never done in a _culture of heroes_[^2]. Among the many reasons, This is simply a math problem: The pool of heroes exists, and while very useful in "start-up" mode, scaling such pool can prove to be very expensive. Organization adopting a _culture of heroes_ are very likely to implement the "Dungeon Masters" anti-pattern. [^3]

Everyone can make a reference to Agile and/or Lean development, but very few speak on the doing "Agile at Scale". I have found great inspiration in how the Spotify folks have done this. I highly recommend watching both [^4] videos [^5]. The key to scaling agile lies in the proper alignment (direction) of teams. What the Spotify folks have done is great food for thought, but it's not meant to be a blue print. Organizations need to define their own structure, one that creates the right value while keeping the negative impact of Conway's Law at a minimum.

## Little's Law: it's all about productivity
While Conway helps us explain the need of an effective organization structure from a high level, we still need to solve the problem of what each team does and how fast they are able to deliver (or not deliver) value. Cross-functional team members are meant to solve the "time to market" problem: Be autonomous, but don't sub-optimize.

As also discussed in Hackernoon [^6], John Little's mathematical proof gives us further insight:

$$ Average Lead Time = \frac{ Average Work in Progress}{Average Throughput}$$

In this context, "Lead Time" is the time for things to get done. Little's law helps us measure how the amount of time it takes to complete tasks (e.g. stories, features, etc) is directly proportional to the number of "things" we are working on. One can only conclude that __for teams to get done faster, they need to commit to work on fewer things__. This suggests an intrinsic need to prioritize "acceptance" criterias to determine MTP (Minimum Testable Product) so that we can start to learn quickly.

![Limit The Work](/assets/img/2020/visualize-work-and-limit-it.png#imageInPost "Limit The work")

## What about the Tools?
One of my favorite quotes from Steve Jobs is:

> "It doesn't make sense to hire smart people and tell them what to do; we hire smart people so they can tell us what to do".

Tools will most likely be needed to design and implement solutions. While we need to codify how to best use these (e.g. handbook of instructions), this is where we allow innovation to happen. Tools will arise as part of the solution, hence we don't start with these enforcement. In my experience, teams need a lab to experiment. The need to productize these tools is in part why an "opinionanted platform" is critical for the sucess of an enterprise.


#### References:
[^1]: [{{ site.data.bookmarks['conwayWikipedia'].title}}]({{site.data.bookmarks['conwayWikipedia'].url}})
[^2]: [The Risk of a Hero Culture](https://futureofsourcing.com/the-risks-of-a-hero-culture)
[^3]: [{{ site.data.bookmarks['dungeonMasterAntipattern'].title}}]({{site.data.bookmarks['dungeonMasterAntipattern'].url}})
[^4]: [{{ site.data.bookmarks['spotifyEngineringCultureVideo1'].title}}]({{site.data.bookmarks['spotifyEngineringCultureVideo1'].url}})
[^5]: [{{ site.data.bookmarks['spotifyEngineringCultureVideo2'].title}}]({{site.data.bookmarks['spotifyEngineringCultureVideo2'].url}})
[^6]: [{{ site.data.bookmarks['conwayAndLittle'].title}}]({{site.data.bookmarks['conwayAndLittle'].url}})