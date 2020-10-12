---
layout: post
title: Documenting Architecture Decisions - ADR
date: '2017-04-28 06:38:00 -04:00'
author: alberto
categories: development
tags:
- Architecture
- documentation
---

As I reviewed the 2017 Thoughtworks technology techniques <a href="https://www.thoughtworks.com/radar/techniques" target="_blank">trends</a>, one cool recommendation was about to "try" the concept of <a href="https://www.thoughtworks.com/radar/techniques/evolutionary-architecture" target="_blank">Lightweight Architecture Decision Records</a>. I must admit, I have been looking for something like this as I'm tired of having note pads everywhere, and I'm really trying to stay away from MS-Word documents and Powerpoints for developers. I have been looking for something lightweight, easy for developers to follow.

I tried <a href="https://github.com/npryce/adr-tools" target="_blank">ADR-tools</a>, and in 10 minutes I was able to document on my personal projects (my pinewood derby project, soon to be added to github). Installing was easy on a Mac:

{% highlight bash  %}
  $ brew install olleolleolle/adr-tools/adr_tools
{% endhighlight %}

Generated pages (markdown templates) already contain the markup needed for github and gitlab (which is probably more than what I need). Running the command:

{% highlight bash  %}
  $ adr list
{% endhighlight %}

Lists all ADR in the local directory. Adding a new ADR is also simple:
{% highlight bash  %}
  $ adr new "Add new Component"
{% endhighlight %}

Creates a page and opens the editor (configured in your EDITOR env variable). Notice the markup is ready to go. Doing the following:

{% highlight bash  %}
  $ adr new -s 2 "Add super new Component"
{% endhighlight %}

Allows you to supersede an ADR and updates the ADR markup pages automatically. This is a sample of a superseded page:
{% highlight markup  %}
  # 5. Add custom contact me page to site
  Date: 28/04/2017

  ## Status
  Superceded by [6. Add mailchimp as default contact me page provider](0006-add-mailchimp-as-default-contact-me-page-provider.md)

  ## Context
  Context here...

  ## Decision
  Decision here...

  ## Consequences
  Consequences here...

{% endhighlight %}

This is a superseding page:
{% highlight markup  %}
  # 6. Add mailchimp as default contact me page provider
  Date: 28/04/2017

  ## Status
  Accepted
  Supercedes [5. Add custom contact me page to site](0005-add-custom-contact-me-page-to-site.md)

  ## Context
  Context here...

  ## Decision
  Decision here...

  ## Consequences
  Consequences here...

{% endhighlight %}

Documenting architecture decisions may be harder than most people think, and I'd concede that this approach may be too simplistic, and almost impossible for monolith apps. In designing and archictecting systems, I'm finding that in the process of "<a href="http://www.businessnewsdaily.com/6848-creativity-vs-innovation.html" target="_blank">innovation and creativity</a>", if I can't describe a system, or tell the story or how we got "there", I'm simply over-architecting.
