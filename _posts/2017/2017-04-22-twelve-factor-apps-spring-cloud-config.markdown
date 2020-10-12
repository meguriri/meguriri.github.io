---
layout: post
title: 'Twelve Factor Apps: Spring Cloud Config'
date: '2017-04-22T11:16:00 -04:00'
author: alberto
categories: development
tags:
- Spring-Cloud-Config
- Spring-Boot
- 12Factor
- Microservices

---

I have been using Spring for many years now and I really like the simplicity and non-invasive model of its features. As I have been implementing <a href="https://12factor.net/" target="_blank">12 Factor</a> applications, I needed to start using a central way to configure the many microservices we are building. This is where ```Spring Cloud Config``` comes to the rescue.

# Spring Cloud Config Architecture
The basic premise is that each application (microservice) can have it's own configuration, but in addition, can receive configuration from a central configuration system - a configuration server. This configuration server is incredibly easy to create with Spring Boot. The following snippets creates the configuration:

{% highlight java linenos %}
package io.cybertech.boot.sample;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}
}
{% endhighlight %}

The following is my example ```application.yml``` file that configures my instance of ```Config Server```:


{% highlight yaml linenos %}
server:
  port: 8888
spring:
  cloud:
    config:
      server:
        encrypt:
          enabled: false
        git:
          uri: ${HOME}/workspace/configuration/sample-boot-demo-configuration

encrypt:
  key-store:
    location: file://${user.home}/workspace/keys/server.jks
    alias: mytestkey
    secret: changeme
    password: letmein
{% endhighlight %}

A few things to notice:
* By default config server will handle encryption following a pre-determined heuristic. More on that later. Suffice to say that the above configuration is meant to make decryption to happen at the client side instead of the server holding the data.
* The key (sample gist shown below) has to be provided for the purposes of decrypting.
* The location of the configuration is stored in a git repository. This so happen to be in my local file system, but can be anywhere. One cool aspect of this is that client applications can use a given version, branch or tag of the configuration within this git repo. Version management of configuration is an interesting topic as "<a href="http://tscm.io/blog-highlyconfigurablesoftware.php" target="_blank">highly configurable</a>" software is an interesting concept to implement while preserving the integrity of the application.
* The documentation assumes that users of this JVM have installed the&nbsp;<a href="http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html" target="_blank">Unlimited Strength JCE</a>.

# Encryption of Values
While there are many ways to protect your configuration server, a common requirement is about protecting the data (configuration) that is stored on file disk as well as to protect it while in transit. This is pretty common for passwords, hostnames, IPs, etc. ```Spring Cloud Config</span>``` offers some options in this space. I recommend asymmetric key encryption for the purposes of protecting the data. The example applications I use on my github repo uses the following bash script to create the key:

{% highlight bash linenos %}
ALIAS_NAME="mytestkey"
KEYSTORE_SECRET="changeme"
KEYSTORE_PASSWORD="letmein"
VALIDITY_TIME=365

echo "Creating server key, valid for $VALIDITY_TIME days"
keytool -genkeypair -alias $ALIAS_NAME -keyalg RSA \
  -dname "CN=Web Server,OU=Unit,O=Organization,L=City,S=State,C=US" \
  -keypass $KEYSTORE_SECRET -keystore server.jks -storepass $KEYSTORE_PASSWORD \
  -validity $VALIDITY_TIME
{% endhighlight %}


Applications using the above ```Spring Config Server``` can use it's properties using the following ```bootstrap.yml``` file:

{% highlight yaml linenos %}
spring:
  application:
    name: survey-service
  cloud:
    config:
      uri: http://localhost:8888
      label: 1.0.0

encrypt:
  keyStore:
    location: file://${user.home}/workspace/keys/server.jks
    alias: mytestkey
    secret: changeme
    password: letmein
{% endhighlight %}

# Lessons Learned:
* I have seen some implementation of "config server" (non Spring solutions) that queries each property remotely. This is dangerous as you can DDOS our own config server on deployment. In ```Spring Cloud Config```, the entire properties file is sent over a single HTTP request.
* I have been using Spring Cloud release Camden.SR6 (Important). While doing so, I found a bug described <a href="https://github.com/spring-cloud/spring-cloud-commons/issues/191" target="_blank">here</a> that was causing issues to the decrypting of properties server side.
* Using defaults is an important strategy. Use it with care.
* You can encrypt/decrypt properties using the endpoint (from the server) located at:
{% highlight bash %}
   localhost:8888/encrypt -d "foo"
{% endhighlight %}
* Properties with the following entry will be decipher as needed:
{% highlight bash %}
   message='{cipher}xxx'
{% endhighlight %}
* Careful between YML and properties files and the {cipher} prefix. As you know, Spring supports property values from "properties" and YAML files. Each are supported a bit differently due to the nature of the file (parsers will interpret them differently). This is important to note as the "single" quote may not be needed in one vs the other.