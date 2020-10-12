---
layout: post
title: 'Git: refusing to merge unrelated histories'
date: '2019-12-01 08:46:00 -05:00'
author: alberto
categories: Development
tags:
- git
---

Most recently, I found the following error during a git operation:
{% highlight bash  %}
  $ git pull
  fatal: refusing to merge unrelated histories
{% endhighlight %}

which led me to a hunt. I found out that since version 2.9.0, git has removed the ability to merge branches with unrelated histories (kudos to the git developers for the obvious message!). This change (and many others) can be found <a href="https://github.com/git/git/blob/master/Documentation/RelNotes/2.9.0.txt#L58-L68" target="_blank">here</a>.

The fix (for me) was found in doing the following:

{% highlight bash  %}
  $ git pull --allow-unrelated-histories
{% endhighlight %}

I suppose that this is relevant if you are concerned about "related" history.
