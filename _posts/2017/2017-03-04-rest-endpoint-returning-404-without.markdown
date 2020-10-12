---
layout: post
title: REST Endpoint returning 404 without Exceptions
date: '2017-03-04T06:34:00.001-05:00'
author: alberto
categories: development
tags:
- Exceptions
- REST
- Spring-Boot
- Spring
---

At some point in your REST endpoint implementation, you will need to return a non 200 response. Now, most implementations show how to implement this using exceptions. While this may seem like a nice approach (from a programming language point of view), exceptions can add <a href="https://shipilev.net/blog/2014/exceptional-performance/" target="_blank">overhead</a> to the running JVM. This translates to ineffective use of resources of the JVM for supporting non 200 responses.

Here is a <a href="https://gist.github.com/albertoaflores/3bc4706e97c158e91117cef7ee13d9fe" target="_blank">gist</a> where you don't use exceptions to return non 200 responses:


{% highlight java linenos %}
@RequestMapping("/{surveyId}")
public Survey findSurvey(@PathVariable String surveyId, HttpServletResponse response) {
	log.info("Querying survey {}", surveyId);
	Survey survey = repository.get(surveyId);
	if (survey == null) {
		log.warn("Survey {} was not found!", surveyId);
		response.setStatus(404);
	}
	return survey;
}

{% endhighlight %}

A few things to note from this snippet:
* This is a Spring Boot implementation (Boot makes things so much simple)
* The ```HttpServletResponse``` is injected by Boot (Spring MVC), no need of additional setup.
* Notice that the body return is null already, so the non 200 response makes more sense.

Please note that this is an implementation specific construct to support non 200 responses. Jersey and others also provide a non exceptional way to handle this scenario. Semantics of what the endpoint does (nouns non verbs, <a href="http://whatisrest.com/rest_constraints/uniform_contract_profile" target="_blank">uniform contracts</a>, <a href="http://whatisrest.com/rest_constraints/interface_uniform_contract" target="_blank">interfaces</a>, etc) are not part of the discussion. This is strictly an implementation description meant to support the abstraction defined by the given REST endpoint.
