---
title: "Maven Helper Dependency Analyzer Plugin for IntelliJ"
date: 2022-05-12T14:49:38+05:30
draft: false
categories: [tech, blog]
tags: [intellij, plugin, maven, java]
description: "Learn analyzing maven dependencies in your projects using an IntelliJ plugin"
author: "Anshul Gautam"
---

This is a very common problem where we come across conflicting dependencies in our maven projects. A dependency can be present as a transitive dependency across several dependencies, and can potentially cause conflict at the runtime. This problem surfaces into a bigger shape when their versions are different. An older version is an outdated version for you may be as it won't offer latest classes bundled inside it. On the other hand it can also be a situation wherein the older version had the classes that you need but the newer major release version had those classes removed. Now you are in trouble.

In this post we will be discussing about an IntelliJ IDEA plugin which helps you find out the conflicting dependencies from your IDE, with a nice and reliable UI.

## The Maven Helper Plugin
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652374246926/Rjc2YWkmA.png)
This [plugin](https://plugins.jetbrains.com/plugin/7179-maven-helper) helps you with:
- Providing an easy way for analyzing and excluding conflicting dependencies
- Gives actions to run/debug maven goals for a module that contains the current file or on the root module
- Action to open terminal at the current maven module path
- Provides actions to run/debug the current test file.

Maven has a goal `mvn dependency:tree` which gives you information on the dependency hierarchy in your maven projects. But the representation is not so good. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652375330527/Th3WFGajG.png)

We will see later in this post how this plugin shows you the same information.

## Installing this plugin
You can install this plugin from the IntelliJ IDEA marketplace, or directly from the official link [here](https://plugins.jetbrains.com/plugin/7179-maven-helper).

For installing it via Market place, in IntelliJ IDEA go to File -> Settings -> Plugins -> Marketplace.

Search for 'Maven Helper' in the dialog, and click on install.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652374490511/o8O6rmpIq.png)

## Using this plugin
Go to your pom.xml. Once you have installed this plugin, you should be able to see an option called Dependency Analyzer. Just click on that.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652374895197/xgMNqSkVQ.png)

Now, let's say that I want to know from where a dependency called `jsr305` is coming from. I will click on 'All Dependencies as Tree' and enter `jsr305` in the search box. Check the Filter checkbox.
And here you go. You will be able to see from what all dependencies you are getting `jsr305` in your project.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652375172271/Rg54IPPQ-.png)

In case you want to see the conflicting dependencies, just click on Conflicts radio button. This will show you the conflicts that you have for the entered artifact.

## Conclusion
This plugin is very useful and a must have for Java developers. I firmly believe this should be present in IntelliJ by default.

If you have never used this plugin before, do give it a try. I am sure you will love this too like I do.