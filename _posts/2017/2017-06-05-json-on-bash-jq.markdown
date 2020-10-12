---
layout: post
title: 'JSON on BASH: JQ'
date: '2017-06-05 20:35:00 -04:00'
author: alberto
categories: development
tags:
- Bash
- REST
- JSON
---

In one of my previous posts, I wrote about underscore-cli for looking at JSON output (e.g. to see the curl response from a REST web service). An alternative to this is <a href="https://stedolan.github.io/jq/" target="_blank">jq</a>, a command line processor for JSON. This CLI allows for simple, yet powerful expressions that make any CLI pro user be productive when interacting with REST services.

Some things about ```jq``` that make it powerful is that it can be used as a filter (that's what ```jq``` really is). This means that you can select which parts of the JSON buffer be returned from the stream so that you can you do your own navigation. Think of SQL for JSON. You can also construct powerful expressions in BASH for parsing JSON without needing python or ruby.

In general ```jq``` is very simple to install and use, with great documentation. For more information, check out&nbsp;<a href="https://stedolan.github.io/jq/tutorial/">https://stedolan.github.io/jq/tutorial/</a>
