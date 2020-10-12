---
layout: post
title: 'Starting a Project: Data Simulation'
date: '2019-11-30 07:42:00 -05:00'
author: alberto
categories: Testing
tags:
- chaos
- spring-Boot
- spring
- testing
excerpt: Data generators are an essential part of a "Digital Twin" strategy. This has always been a part of every problem domain&#58; We need to create data that reflects production behavior, and we need to be able to reproduce a behavior that happens in the past. Notice that this is not just about data generation, but data simulation. This entry is about some initial thoughts on this topic.
---

Sometimes, there is a need to generate data to test your code. While "<a href="https://martinfowler.com/articles/qa-in-production.html" target="_blank">testing in production</a>" is a sweet spot to be, there are times when you need to "force" (stub) data that reflects certain business phenomena in production.

This does not mean "<a href="https://principlesofchaos.org/?lang=ENcontent" target="_blank">chaos engineering</a>" but rather a finite set that can be ingested into a suite of integration tests to increase confidence in code change.

For example, creating a number of threads to generate load can be done trivially with Apache Bench (ab), I'm looking for a scenario builder with lots of traffic data. I'm referring to the "needle in the haystack" scenario builder.

I started with some ideas on how to create multiple "stubs" in a thread pool so that I can force concurrent payloads to hit a service to test. Using the Spring Framework and Spring Boot, this task became super simple. I ended up with the following:

{% highlight java linenos %}
   @Async("myThreadPoolTaskExecutor")
   public Object createPayload() {
     // return payload pojo
   }
{% endhighlight %}

and the following TaskExecutor bean:

{% highlight java linenos %}
   @Bean(name = "myThreadPoolTaskExecutor")
   public TaskExecutor threadPoolTaskExecutor() {
      ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
      executor.setCorePoolSize(4);
      executor.setMaxPoolSize(4);
      executor.setThreadNamePrefix("executor");
      executor.initialize();
      return executor;
    }
{% endhighlight %}

Of course, this is only a start with the following potential test:
* "Transaction boundaries" defined in my code.
* Verify the "concurrency" setup.
* Create a set of scenarios (the needle in the haystack)

I'll update more on this journey as I add more to the data generator project I have been creating.
