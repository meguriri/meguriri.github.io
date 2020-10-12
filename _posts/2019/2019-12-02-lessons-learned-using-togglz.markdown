---
layout: post
title: Lessons Learned using Togglz
date: '2019-12-02 17:35:00 -05:00'
author: alberto
categories: Development
tags:
- togglz
- spring-boot
- spring
---

Feature Toggles (a.k.a. [Feature Flags](https://www.martinfowler.com/articles/feature-toggles.html){:target="_blank"}) is a very powerful pattern to test and introduce new features in a production environment. I started to experiment with <a href="https://www.togglz.org/documentation/spring-boot-starter.html" target="_blank">Togglz</a> as a OSS library to implement what I needed. While I used Spring-Boot to demostrate my use of Togglz, this can be done with any other framework.

For Spring-Boot, the documentation says all you need is to add the following:

{% highlight xml linenos %}
  <dependency>
    <groupId>org.togglz</groupId>
    <artifactId>togglz-spring-boot-starter</artifactId>
    <version>2.6.1.Final</version>
  </dependency>
{% endhighlight %}

This is not completely true as the following error occurs:

{% highlight log %}
  [ERROR] contextLoads  Time elapsed: 0 s  <<< ERROR!
  java.lang.NoClassDefFoundError:
  org/springframework/web/servlet/config/annotation/WebMvcConfigurerAdapter
{% endhighlight %}

It appears you need to use the following dependency:

{% highlight xml linenos %}
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
{% endhighlight %}

This makes the code to compile without problems. Of course, I also needed the console module, so I added:

{% highlight xml linenos %}
  <dependency>
    <groupId>org.togglz</groupId>
    <artifactId>togglz-console</artifactId>
    <version>2.6.1.Final</version>
  </dependency>
{% endhighlight %}

This allowed me to have a working app with features enabled/disabled (very quickly I may add). Next was unto setting up a feature flag. This was very easy with the following properties:

{% highlight properties linenos %}
  togglz.console.enabled=true
  togglz.console.path=/toggles
  togglz.console.secured=false
  togglz.console.use-management-port=false
  togglz.features.FAKE_JMS_LOAD.label=Fake JMS Load
  togglz.features.FAKE_JMS_LOAD.enabled=true
{% endhighlight %}

Notice that I had to change the configuration to also not use the management port. This is described <a href="https://github.com/togglz/togglz/issues/203" target="_blank">here</a>.

A few things I would like to see in future distributions of Togglz:

* Given that Spring-Boot is simple to use, I think we should be able to have a way to add the owner of a given toggle. This is not possible with the existing API.
* In general, Togglz does not have a way to add more description to the flag. This seems like an essential feature, and I can see how much info can get lost.

In general, this seems like a good simple implementation of the "Feature Flag" pattern that provides a nice UI to non-technical folks to manage. It's definitely a great start, but I can see having the need to have more features.