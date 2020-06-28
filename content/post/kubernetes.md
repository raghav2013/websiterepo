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

In this post, I will provide example of a Spring Boot application deployed in kubernetes cluster as a Service that exposes Prometheus metrics that will be consumed by prometheus operator
also deployed as a Kubernetes Service

Please note that basic knowledge of docker, kubernetes deployment, service and replicas are required to follow this tutorial


Let's do the following steps

1. Build a basic spring boot web application

2. Expose spring boot actuator endpoint

3. Build a docker image of the spring boot application

4. Upload the docker image to docker hub

5. Set up single node cluster in minikube

6. Define the kubernetes deployment and service objects

7. Do a helm install of prometheus operator

8. Configure a ServiceMonitor object that will monitor the spring boot service

9. Access the prometheus metrics using the prometheus operator service

The code for all the above steps is available at https://github.com/raghav2013/prometheus_demo



* Build a basic spring boot web application. We are using the prometheus library from io.micrometer

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- Micrometer Prometheus registry -->
		<dependency>
			<groupId>io.micrometer</groupId>
			<artifactId>micrometer-registry-prometheus</artifactId>
		</dependency>

* Expose spring boot actuator endpoint

For simplicity, I am exposing all the metrics by using the following setting in application.properties. Be sure to change
this before you deploy to production

		management.endpoints.web.exposure.include=*


* Build a docker image of the spring boot application

		docker build -t <docker-hub-username>/repo-name:tag
		example : docker build -t docker062218/spring-boot-actuator-enabled-demo:v1


* Upload the docker image to docker hub

After building and tagging the image, you can push to docker hub using docker push command. Before you can push, you
need to login to docker hub or any other image registry where you store your images

		docker login --username=yourhubusername --password=************ https://index.docker.io/v1/


You can choose not to pass in your password through command line and use a secure option

After you login, you can push the spring boot application image using docker push

        example: docker push docker062218/spring-boot-actuator-enabled-demo:tagname

* Set up single node cluster in minikube, install kubectl

You can follow the guide at https://kubernetes.io/docs/setup/learning-environment/minikube/ to install minikube 
<br>
For kubectl, please follow - https://kubernetes.io/docs/tasks/tools/install-kubectl/



* Define the kubernetes deployment and service objects


The deployment and service object definition can be found under kube-deploy folder at https://github.com/raghav2013/prometheus_demo

        **Deployment definition**

        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: spring-boot-actuator-demo
          labels:
            app: springbootactuator
        spec:
          replicas: 3
          selector:
            matchLabels:
              app: springbootactuator
          template:
            metadata:
              labels:
                app: springbootactuator
            spec:
              containers:
              - name: springbootactuator
                image: docker062218/spring-boot-actuator-enabled-demo:v1
                ports:
                - name: web
                  containerPort: 8080


         **Service Definition**

         apiVersion: v1
         kind: Service
         metadata:
           name: app-service
           labels:
             app: app-service
         spec:
           selector:
             app: springbootactuator
           ports:
             - name: web
               port: 9000
               targetPort: 8080
           type: NodePort



We deploy the deployment and service objects into cluster by using the following kubectl commands

1. kubectl apply -f deployment.yaml
2. kubectl apply -f app-service.yaml


Here we are running the service using NodePort. You can access the service by running the following command

    minikube service app-service

* Do a helm install of prometheus operator

Please follow the steps below to ensure you have enough privilege to administer the minikube

    kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=system:serviceaccount:kube-system:default

    helm repo add stable https://kubernetes-charts.storage.googleapis.com/

    helm install  prometheus-operator stable/prometheus-operator


* Configure a ServiceMonitor object that will monitor the spring boot service

    We have defined a ServiceMonitor to monitor our app-service


        apiVersion: monitoring.coreos.com/v1
        kind: ServiceMonitor
        metadata:
          name: prometheus-operator-app-service
          labels:
            release: prometheus-operator-1590907740
        spec:
          selector:
            matchLabels:
              app: app-service
          endpoints:
          - port: web
            path: '/actuator/prometheus'
            interval: 10s
            honorLabels: true

* Access the prometheus metrics using the prometheus operator service

    Do a port forward on the prometheus operator service - as

    kubectl port-forward svc/prometheus-operated 9090:9090

    Access the metrics at http://localhost:9090/targets









