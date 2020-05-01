---
author: "Raghavendran"
date: 2020-23-02
title: Spring boot multiple datasource configuration
description: "Spring boot multiple datasource configuration"
topics:
  - topic 1
tags:
#  - one
#  - two
draft: false
---

For few applications, we come across a requirement to connect to a variety of data sources (example multiple oracle datasources , or a combination of Oracle, MySql data sources etc)

Spring does offer automatic configuration by providing sensible defaults. One of the challenges,  I faced is how would we mainitain different configuration for different datasources

In this case, you have to define the configuration and Spring boot would detect that configuration and you have the flexiblity to maintain different configurations

Below is an example of a spring boot application that connects to two different postgres databases

The configuration classes under com.simplidata.multidbconfig.config provide for configuring multiple datasouces

The code can be downloaded from github at https://github.com/raghav2013/spring-multi-db-config
    