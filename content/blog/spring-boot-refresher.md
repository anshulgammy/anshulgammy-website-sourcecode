---
title: "Spring Boot Refresher"
date: 2022-03-08T18:19:05+05:30
draft: false
categories: [tech, blog]
tags: [spring boot, java, programming]
description: "Refresh yourself with basic concepts from Spring Boot"
author: "Anshul Gautam"
---
This article helps to get an overview of Spring Boot. This will also be helpful to refresh your basic concepts on this topic quickly.

![Spring Boot Refresher Banner](/blog/spring-boot-refresher.jpg "Spring Boot Refresher Banner")

## 🎯 What is Spring Boot?
Spring Boot is comprised of two separate words; '**Spring**' and '**Boot**'.

'**Spring**' is the Spring Framework. This is the Java Application Framework which helps you write Java Enterprise Applications.

'**Boot**' is Bootstrap.
Spring Boot helps you Bootstrap a Spring Application.

Well, the official definition goes something like this:
*Spring Boot makes it easy to create stand-alone, production-grade Spring based applications that you can "just run".*

In a typical Spring Framework, there are lots of configuration needed to be done. With Spring Boot, this is no longer the case. You can run the Spring Application with very less, or no configuration at all, and can easily run as a standalone application. 

## 🎯 Spring vs Spring Boot
Spring started of as a tool to provide dependency injection functionality. But today, Spring has grown into a lot more. It is a huge Application Framework now, which uses programming and configuration model. This has invites few problems with Spring Framework, like it involves multiple setup/configuration steps. It may lead to multiple build and deploy steps.

Spring Boot tries to cater to the functionalities provided by the Spring Framework, with very easy and minimum abstracted configuration steps. It does the most common configuration for you out of the box. In case your application needs some extra configuration, you can do that as well. 
In short, Spring Boot is:
- Opinionated
- Convention over Configuration
- Stand-alone
- Production ready

## 🎯 Creating Spring Boot Application with Maven
You must be aware that Maven is build and dependency management tool. For creating Spring Boot Application via Maven, you need to add a parent project in the pom.xml:

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>${latest.version}</version>
</parent>
  
``` 
Here `${latest.version}` corresponds to the latest version of this parent project being offered by the Spring.

This parent project provides opinionated configuration for your child project. This ensures that the bare minimum configuration needed for your Spring Boot project is already provided to you and you don't need to work much on that part. This is analogous to child inheriting the property defined in the parent.
You will be focusing on writing business code mostly.

## 🎯 Creating Spring Boot Application with Spring Initializr
Easiest way to create Spring Boot application from scratch is by going to [Spring Initializr](https://start.spring.io/). This portal will help you choose what all dependencies and their versions you would want in your project, and will create a scratch project for you. You can download the project zip file and start adding on your code.

## 🎯 Starting Class for your Spring Boot Application

```
@SpringBootApplication
public class FirstSpringBootApplication {

    public static void main(String args[]) {
        SpringApplication.run(FirstSpringBootApplication.class, args);
    }
}
``` 
The `@SpringBootApplication` annotated on the class acts as the starting point in you application.
This does a lot of thing in the background for us:
- Sets up default configuration
- Starts Spring application context
- Performs class path scan
This will also start Tomcat server in case you have added Spring Web. Spring Web provides embedded Tomcat server out of the box.

## 🎯 Why Embedded Tomcat Server in Spring Boot Application
Having an embedded Tomcat server in your project is helpful in few ways:
- Convenience
- Servlet container config becomes part of application config
- Stand-alone application
- Useful for microservices architecture
Can we change embedded Tomcat server to a some different servlet container? Yes we can.

## 🎯 Customizing Spring Boot
You can configure using a properties file. 
You keep ```application.properties``` or ```application.yml``` file in your resources folder.
You can do the configuration by providing key value pairs in the properties file.
Eg:
```
server.port=8765
```
The above key value pair in our property file overrides the server port from 8080 to 8765.

Properties file is one of the easiest way to configure your application, and override the defaults that Sprint Boot provides to you out of the box.
