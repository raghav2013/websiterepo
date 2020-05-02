---
author: "Raghavendran"
date: 2020-02-02
title: Spring boot application monitoring in Kubernetes
description: "Spring boot application monitoring in Kubernetes using Prometheus"
topics:
  - topic 1
tags:
#  - one
#  - two
draft: false
---


Spring boot is widely popular in microservices world as it integrates and provides a number of services like data source, security  and also provides the flexibility to override the defaults 

Docker provides a uniform  environment to run the applications. So these days, we see the application be it Springboot  or Dot NET being packaged as docker images

The next step on application management  is to provide an environment for automating deployment, scaling, and management of containerized applications. Kubernetes fits the bill quite nicely here

Kubernetes is supported  on Azure, AWS and other popular cloud platforms 

We often feel the need to monitor the application we deploy so as to gain useful insights on performance, and ultimately focus on improvement

In this article, I will provide example of a Spring Boot application deployed in kubernetes cluster as a Service that exposes Prometheus metrics that will be consumed by prometheus montitor also deployed as a Kubernetes Service


Components that we need to build

1. Basic Spring boot application that includes spring bott actuator and prometheus dependency

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- Micrometer Prometheus registry -->
		<dependency>
			<groupId>io.micrometer</groupId>
			<artifactId>micrometer-registry-prometheus</artifactId>
		</dependency>

Code for the above can be found at https://github.com/raghav2013/prometheus_demo

2. Docker File that packages the application 

3. Define Kubernetes Service and Deployment files for deployment to Kubernetes cluster (minikube or Cloud) 

4. Deploy Prometheus Kubernetes service with appropriate configuration to access the spring boot application endpoint








