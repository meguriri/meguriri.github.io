---
layout: post
title: 'Vertical Slicing'
date: '2020-06-17 10:00:00 -04:00'
author: alberto
categories: Transformation
tags:
- Agile
- Discovery
- UX
- Requirements
excerpt: When we talk about User stories, we often assume that everyone will quickly write 'As a user A, I want to do X so that I can do Y'. Anyone who practices Agile in some fashion knows that finding those well-defined stories takes effort.  In practice, I have found that most approaches to obtain such stories end up in the adoption of processes that resemble some waterfall process. Vertical Slicing is a technique that can help focus on the discovery of requirements while always deliverying value.
---
There are three fundamental Laws in Sofware development. These are:
* Conway's Law
* Ziv's Law
* Humprey's Law

In my previous post, I spoke about [Conway's Law]({% post_url 2020/2020-06-16-introducing-conways-law %}). Today, I'll reference the other two as they are relevant to the topic of "Vertical Slicing" to which I'll present as an approach to mitigate the effects of these laws.

## Dissecting "Water-Scrum-Fall"
![Waterfall vs Agile](/assets/img/2020/water-scrum-fall.jpg#imageInPost "Waterfall vs Agile")

"Water-Scrum-Fall" is largely credited to Dave West (Forrester Research) in a 2011 article[^1]. West argues the reality of most enterprises and how the term is the sum of the following:
* "Water" as the way to gather requirements.
* "Scrum" as the way to build software.
* "Fall" as the way we release software.

Most organizations focus on the "scrum" part of the organization, while failing to measure the "requirements" gathering process as well as the "release" process. Today, I'll focus on the "requirements" process.

Large organizations can benefit of this discussion by discussing (in retrospective meetings) how "agile" and "transformational" is their "requirements" process. We can only fix what we recognize and we talk about.

## From Theory to Practice
In large organizations, it's simpler to see the effects of Ziv's law:

> "Software development is unpredictable and that the documented artifacts such as specifications and requirements will never be fully understood."

Ziv argues that requirements, no matter how much time we spend on it, it's meant to change. I have seen how this is true, even in critical systems where change is necessary and the risk of failure is high. Let's think about this for a moment: __The issue with waterfall, was never about the building of software, but about the understanding of requirements__. Additionally, consider Humprey Law:

> "The user will never know what they want until after the system is in production (maybe not even then)"

This is also referred to as "I'll know it when I see it" syndrome. From Ziv and Humprey, we can conclude that requirements gathering is in great need of "transformation".

## Vertical Slices: A simpler path to MVP
Building software can be described in the following paradigm:

![Vertical Slicing](/assets/img/2020/vertical-slice.png#imageInPost "Vertical Slicing in Lean")

As we build software, we tend to traverse this triangle in either horizontal or vertical patterns. Let's evaluate each section of this journey:

* **Functionality**: If we think of the way deliver software, most engineers will agree to a solution when completion is "technically feasible" (bottom of the pyramid). This of course, is the minimum we should expect of any solution (e.g. it has to work).
* **Reliability**: Another level of completion is when a software deliverable is "useful". This is yet another level of completion where software delivers minimum value.
* **Usable**: A third level of completion that refers to the "usability", higher than "useful" only because now the software starts to introduce __effectiveness__ in solving a problem domain.
* **Beauty**: The highest level of software delivery where we always want to be, but hardly get there: Users loving the experience of using the sofware we build - _a joyful experience!_

The concept of _Vertical Slicing_ is very simple: In order to build software, we need to determine and agree with all stakeholders on elements of the software (e.g. a vertical slice - feature) that will likely touch multiple architectural layers to implement a change. When a slice is done, the software system is clearly more valuable to the users, and joyful experiences are delivered in increments. Thus we look to slice vertically instead of horizontally as much as we can.

Teams working with "Vertical slicing" must also practice the following habits:

* Include value description explicitly in backlog.
* Have more conversation about value (empathy).
* Try not to accidentally build low-value changes.
* Look for feedback early in the process.
* Become predictable in delivering value.

Learning to decompose ideas into vertical slices requires a different skill, one that can be learned in a short period of time. There are many patterns of these [^2] (which I'll write in future entries), for now, here are a few set of principles to keep in mind:

![Vertical Slicing Types](/assets/img/2020/vertical-slices-types.jpeg#imageInPost "Vertical Slicing Types")

## How does this work at scale?
The short answer is "It does!", however this becomes an "Organizational Structure" issue and not so much an architecture issue. While your architecture hopefully helps (e.g. microservices), it's important to understand that such is not required. In practice, your organization structure will build the kind of decoupling that microservices needs (based on Conway's law).

There are two primary ways[^3] to organize team to practice _Vertical Slices_:
* **Feature teams**: cross-functional teams that deliver a complete slice of value (vertical slice) over some or all of the product.
* **Components teams**: these are teams that focus on a particular component, architectural layer, or technology. Delivering a complete slice of value requires the coordination of multiple teams.

This is the "art" of team organization: While we favor **feature teams**, there are times when you need **component teams**. This need is reduced as business alignment, automation and platform continue to evolve. In fact, as Josh McKenty argues:

> "Platforms can allow businesses to cultivate a sense of ‘we’re all in this together,’ in which everyone is respected, treated with mutual regard, and can clean up each other’s messes – regardless of whether they created the mess in the first place,"

This is, among many other reasons, why opinionated platforms are key to both collaboration and effectiveness.

## Conclusion
Large organization can benefits from establishing efforts to modernize the "requirements gathering process". And while any new process (such as _Vertical Slicing_) may be useful, it's not the only needed change in the organization. It also requires organizational changes (people) to support such process. The use of an opinionated platform (tools) are there to support **feature teams** that can focus on _vertical slices_ that deliver value.

#### References
[^1]: [Water-Scrum-fall is the reality of agile](https://sdtimes.com/agile/analyst-watch-water-scrum-fall-is-the-reality-of-agile/)
[^2]: [User Stories Making the Vertical Slice](https://appliedframeworks.com/user-stories-making-the-vertical-slice/)
[^3]: [Vertical Slicing and Scale](https://agileforall.com/vertical-slices-and-scale/)
