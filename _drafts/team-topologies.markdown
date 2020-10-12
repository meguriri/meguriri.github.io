---
layout: post
title: 'Team Topologies'
date: '2020-07-18 13:00:00 -04:00'
author: alberto
categories: transformation
tags:
- teams
- lean
excerpt: 'My notes from my reading on the book "Team Topologies". This books provides an excellent summary of how I have been organizing teams in recent years. From Spotify, to Pivotal and other great teams, this book has great content that will allow teams that need to
scale organizations to perform at high levels.'
---

It's been a while since I've written about a book that describes a lot of my current thinking in terms of organizing "high performing" teams. Team Topologies


## Reverse Conway
I already discussed about how the effects of Conway's Law is inevitable, but can we

James Lewis (Thoughtworks) whereby an organization focuses on organizing team structures to match the architecture they want the system to exhibit, rather than expecting teams to follow a mandated architectural design

Thinking of software architecture as a standalone concept that can be designed in isolation and then implemented by multiple teams is fundamentally wrong.

This gap between teams and architectural structures is visible across all types of architectures independent of the software stack or design choice. Specifically, that's why the monolith need to broken down. In particular any indivisible software part that exceeds the "cognizant capacity" of any one team while keeping a team focused.

## Cognitive Load
How much data (or information) can a person or team hold? Do teams measure how much is too much? When Cognizant load is not considered, teams can run thin forcing them to "context switch" often. Naturally, this leads to single points of failures and friction.

The number of services for which a product team is responsible (demands on the team) typically grows overtime. The development of new services is often planned as if the team has zero responsibility and zero cognitive load. This neglect is problematic, the team still needs to fix bugs and enhancements. Ultimately, the team becomes a delivery bottlenecks as their cognitive capacity has exceeded leading to delays, QA issues, and decrease in team members motivation.   We need to put the team first, advocating for restricting their cognitive load.

Overall, the organizational design that optimizes the "flow of change" and feedback from running system. This requires restricting cognitive load on teams and explicitly designing the intercommunicatinos between them, to help produce the systems architecture we need based on Conway's Law.

