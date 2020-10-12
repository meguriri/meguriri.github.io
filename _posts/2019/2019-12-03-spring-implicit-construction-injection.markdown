---
layout: post
title: 'Spring: Implicit Construction Injection'
date: '2019-12-03 12:29:00 -04:00'
author: alberto
categories: Development
tags:
- spring
---
Over the years, Spring has made a number of enhancements to improve the developer UX. I realize some of these can be too trivial to notice. Today, I present one enhancement that, while minor, may save some from undesirable behavior. This is available since the 4.3 Spring release and it's called "_Implicit Construction Injection_".

As you know, constructor injection is a preferred pattern to add code to your dependency definition. For example, we can do the following:

{% highlight java linenos %}
@Component
public class TaskManager {
  private ServiceReviewPublisher publisher;

 /**
  * This constructor will need dependency injection
  */
  @Autowired
  public TaskManager(ServiceReviewPublisher publisher) {
    this.publisher = publisher;
  }
}
{% endhighlight %}

Notice that historically, developers would have to declare the constructor with an ```  @Autowired``` annotation. This was so that the code can do proper injection. As of Spring 4.3, that is no longer required. This will make the new code be this way:

{% highlight java linenos %}
@Component
public class TaskManager {
  private ServiceReviewPublisher publisher;

 /**
  * This constructor have dependency injection because of the @Component stereotype.
  */
  public TaskManager(ServiceReviewPublisher publisher) {
    this.publisher = publisher;
  }
}
{% endhighlight %}

This makes this class one step closer to be a POJO.