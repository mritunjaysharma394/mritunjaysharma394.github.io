---
layout: post
title:  "Introduction to Thanos!"
summary: A beginner-friendly introduction to Thanos.
author: Mritunjay Sharma
date: '2020-10-02 23:32:23 +0530'
category: Thanos
thumbnail: /assets/img/posts/thanos-stacked-color.png
keywords: Thanos, go, sre, beginner
permalink: /blog/intro-to-thanos
---

You can hate me for this but I am not an Avengers Series fan and I am not going to introduce you to the Thanos we meet there :stuck_out_tongue:

Which Thanos are we going to talk about then? And do you really need to know anything as pre-requisites to understand what you will be learning in this blog? Nope :)

#Before Thanos, let's discuss Monitoring

There's this interesting saying-
> "Running a product without any monitoring is like NOT running the product at all"

Yes, that's right! Before we discuss 'what', let's discuss 'why'!

## Let's make you run a Startup

Now, let us assume that you are reading this blog and you are running a startup that employs multiple applications and is being used by thousands of users. Wow, already feeling amazing? :sunglasses: Yes, you should but let's see one fine day - one of your services breaks and multiple users start facing some kind of outage? (*I pray that it doesn't happen with you on weekend*) What will happen then? Well, I don't know exactly but it's a possibility that the game of pressures will surely begin which you will transfer to the Engineering Team and the Team will be passing it to you and you just can't see hell breaking loose everywhere whether it's your team or it's the users. 

So what can you do to avoid this kind of situation or at least deal better? Yes, you are right - Monitoring!
Won't the situation become really better if you know it in advance about the errors that can come up with the users? You and your engineering team will love it, in fact!
Monitoring is therefore very important. It not only helps you get alerted but also helps you observe trends that can drive your decisions, low-level debugging, gaining insights and automation as well.

That's all about Monitoring - but, hey, who will help you monitor your applications?   

##Prometheus: Anyone said Monitoring? 

You all wondering this blog is about Thanos but we are discussing Monitoring, Startup and now even Prometheus? 

Relax, relax, relax. We are going to come to Thanos but we need to discuss all this before meeting Thanos. You know meeting Thanos is not easy, right? Prometheus says you meet me and it's a promise I will let you meet Thanos! 

[Prometheus](https://prometheus.io/) is your leading open-source monitoring and alerting solution and we are going to discuss in brief what it does and what are the certain limitations that led to Thanos? 

Prometheus in itself is a very vast solution to understand and this blog being beginner-friendly will not try to go into jargons. In plain simple words - Prometheus tries to focus on one thing and it does that extremely well. It offers a data model and a query language that helps you analyse how your applications are performing. 

So far so good but now we discuss few limitations in Prometheus that led to Thanos: 

* Imagine the petabytes of data being accumulated? If we just used Prometheus, at best, we can have retention of one week of data - What if the Engineering team needs data of a year back? Prometheus, in itself, was not enough for the Retention of data.

* Also, if we are trying to retain the data, can it be done in a cost-effective way? What about the sacrifice that we have to make with responsive query times? 

* Suppose, different Prometheus servers are up and you want metrics from a single query API? This again became a strong reason behind the birth of Thanos.

*  One important feature of Prometheus is its High Availability(HA) and it's great but you see that if we get the capability to merge the replicated data collected via its HA setups?

All these problems led to the birth of Thanos!

#Thanos 

Let's talk about Thanos now! It began at [Improbable](http://improbable.io/), a London-based company, by **Bartek Plotka and Fabian Reinartz**, and is now a full-fledged Open Source Project following the Cloud Native Computing Foundation Governance structure. 

So, what does Thanos does? Yes, it solves the above problems but how? Let's figure it out by discussing Thanos architecture: 

* Solving Global Query View + HA: If we talk in simple words - Thanos is built on top of Prometheus and to get a global view on top of an existing Prometheus setup, we need to interconnect it.
 
>The Thanos Sidecar component does exactly what a side-car means. It is deployed next to each running Prometheus server and acts as a proxy. 

![Global Query View+HA Architecture](https://dev-to-uploads.s3.amazonaws.com/i/m9favvx7l2ac2tydcg4s.jpg)

<figcaption>Thanos Side-Car component (Pic Source: https://improbable.io/blog/thanos-prometheus-at-scale)</figcaption>

* Retention of historical Data using Object Data Storage Services (Like S3, Azure, GCP, etc) 

![Retention of historical Data using Object Data Storage Services](https://dev-to-uploads.s3.amazonaws.com/i/ct2xyquyo0y8eg3bgozj.jpg)

<figcaption>Thanos using Object Data Storage Services (Pic Source: https://improbable.io/blog/thanos-prometheus-at-scale)</figcaption>

* Downsampling: Yes, the historical data can be huge and this also means that you need to worry about the complexity of algorithms as it will affect the query times. Thanos thus uses the technique of Downsampling (A process that reduces the sampling rate of signal). Thanos will help Prometheus achieve this which Prometheus alone cannot do. 

![Downsampling](https://dev-to-uploads.s3.amazonaws.com/i/7yz1hx8b002zm2gwar7o.jpg)

<figcaption>Thanos Downsampling (Pic Source: https://improbable.io/blog/thanos-prometheus-at-scale)</figcaption>

* Ruler class: Recording rules is important to reduce cost, latency and complexity of the monitoring stack. Prometheus although offers it in itself but there are few cases where it has limitations like:
   * alerting when two or more clusters are down.
   * Rules that are not in the scope of Prometheus Data Retention.  
   * The case of storing all the rules and alerts at a single place.

To solve all these cases, Thanos offers a separate component called `Ruler` that evaluates rules and alerts of Thanos queries. 

![Thanos-Ruler](https://dev-to-uploads.s3.amazonaws.com/i/ho8de2816k6wpb79y9ml.jpg)

<figcaption>Thanos Ruler Component (Pic Source: https://improbable.io/blog/thanos-prometheus-at-scale)</figcaption>

So this was a basic introduction that I tried to give you all about Thanos and I hope that this introduction will invoke interests in you to dive deeper in Thanos. 

I am attaching links of a few excellent blogs, tutorials and videos that acted as a reference for me and will help you as well further: 

###Links to Blogs

* [Multi-Cluster Monitoring] (https://banzaicloud.com/blog/multi-cluster-monitoring/)
* [HA Kubernetes Monitoring using Prometheus and Thanos]
(https://medium.com/the-metricfire-blog/ha-kubernetes-monitoring-using-prometheus-and-thanos-48e8016abcf7)
* [Spinning up a highly available Prometheus setup with Thanos] (https://appfleet.com/blog/progressive-feature-driven-delivery-with-flagger/)

###Links to Videos

* [Thanos: Prometheus at Scale! - Lucas Servén Marín, Bartlomiej Plotka (DevConf 2020)] (https://www.youtube.com/watch?v=q9j8vpgFkoY)
* [Intro to Thanos: Scale Your Prometheus Monitoring With Ease - Lucas Serven & Dominic Green] (https://www.youtube.com/watch?v=m0JgWlTc60Q)

###Link to Interactive Tutorial (No external setup required)

* https://www.katacoda.com/thanos/courses/thanos

**PS:** 

Thank you so much for reading the blog!:smiley: 

Being a beginner myself with Thanos, I thought learning anything new will be better by writing about it and teaching it to others :sparkles:

However, there is a possibility that I might have made inadvertent errors or missed something. 

Please feel free to improve the blog by posting your feedback or you can reach out to me on Twitter :dizzy: (https://twitter.com/mritunjay394)





