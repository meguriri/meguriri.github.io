---
layout: post
title: Managing Service Brokers in CF
date: '2015-04-02T05:00: -04:00'
author: alberto
categories: development
tags:
- CloudFoundry
- Brokers
- Brooklyn

---

The existing Cloud Foundry (CF) architecture includes a layer to support "Service Brokers":

![CF Components](/assets/img/2015/cf_architecture_block.png#imageInPost "CF Components")

As such, CF includes a registry to manage "Service Broker" implementations. You can use this registry to configure the availability of services (including plans within a service) to different organizations. This is among the things that makes CloudFoundry a very powerful platform. I wrote a simple broker that should give you a quick overview of how to use it with CF. This is found at <a href="https://github.com/albertoaflores/dummy-cf-service-broker">https://github.com/albertoaflores/dummy-cf-service-broker</a>

# Introduction
You need to be an <b>admin</b> for this. This is good, otherwise anyone can alter the services added to your CF instances. Furthermore, only an admin user should be allowed to configure what services are available to each organization.

While describing the architecture of this "service model" is beyond the scope of this post, managing service brokers includes some basic understanding of both terminology and architecture. Please refer to the official docs found <a href="http://docs.cloudfoundry.org/services/overview.html" rel="nofollow" target="_blank">here</a>. In general, a Service Broker must implement a REST API described <a href="http://docs.cloudfoundry.org/services/api.html" rel="nofollow" target="_blank">here</a>.This way, CF can interact with the service being brokered and inject it to consuming applications. This is why projects like Apache Brooklyn makes so much sense. This will be the subject of a different entry post to be made in the future.

# The Sample Service Broker
For the purposes of this post, I'll be using a sample app that also contains an implementation of the "Service Broker" API (see github repo above). Notice that this app relies on a Spring Boot Project to implement most of the broker API. Furthermore, I have configured it so that the password in this dummy service broker has a dummy password (see above for the project repo).

I have pushed this app in CF itself, however you should be able to use this app and deployed it anywhere you want to, as CF only interacts with brokers via the exposed endpoints that implement the API.

# Managing the Broker
Once you have your app installed somewhere (in my case in a CF instance), the following commands are essentially what you be using:

* ```cf service-brokers```
* ```cf create-service-broker```
* ```cf service-access```
* ```cf enable-service-access```

Notice that you'll only be using mainly 2 commands to get your broker available. The other ones are for verification purposes (query them in the registry). The ```cf service-brokers``` command lists all existing service brokers available in your CF instance:

![CF Available Brokers](/assets/img/2015/cf-available-brokers.png#imageInPost "CF Available Brokers")

Notice that it describes the location of where the broker is hosted. Adding the suffix of /v2/catalog to those urls should tell you more about whether the endpoints are properly implemented.

The next step is to create the a record of this service broker in the CF registry. This can be done with the ```create-service-broker``` command:

{% highlight bash  %}
   cf create-service-broker ms-sample-broker \
                            user password \
                            [URL of where your service broker is deployed]
{% endhighlight %}

You can verify this with the previous command:

![CF Updated Brokers](/assets/img/2015/cf-available-brokers-updated.png#imageInPost "CF Updated Brokers")

Now that CF knows about this service broker, we learn something interesting when using the ```cf service-access``` command:

![CF Service Access](/assets/img/2015/cf-service-access.png#imageInPost "CF Service Access")

Notice that the newly added service is not accessible. This is by design! So now you need to configure which organizations will have access to this service defined by this service broker. You do this with the ```cf enable-service-access``` command:

![CF Service Access Enabled](/assets/img/2015/cf-service-access-enabled.png#imageInPost "CF Service Access Enabled")

Notice that now the ```Basic``` plan of the ```ms-sample-service``` is available to the ```fe-federal``` organization. Also notice that the ```-p Basic``` and ```-o fe-federal``` was optional. Omitting these would have caused all plans to be accessible to all organizations in this CF instance. Now, these are available in the marketplace:

![CF Marketplace](/assets/img/2015/cf-marketplace-view.png#imageInPost "CF Marketplace")

In the same fashion, there are commands to remove brokers, change plan access levels, etc.

I hope these helps!