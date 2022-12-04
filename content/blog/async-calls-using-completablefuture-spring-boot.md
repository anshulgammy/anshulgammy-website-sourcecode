---
title: "Async Method Calls using CompletableFuture in Spring Boot"
date: 2022-04-08T19:49:38+05:30
draft: false
categories: [tech, blog]
tags: [reactive, programming, java, completablefuture, spring boot]
description: "Configure ThreadPoolTaskExecutor in Spring Boot application for achieving async method calls"
author: "Anshul Gautam"
---

In my previous [post](/blog/reactive-programming-using-completable-future), I discussed on `CompletableFuture` provided by Java concurrency API in **JDK 8** onwards.

In this article, I want to discuss on using CompletableFuture in a Spring Boot application.

## 🎯 ThreadPoolTaskExecutor in Spring Boot
Spring offers ThreadPoolTaskExecutor as a Java bean which is an abstraction of ThreadPoolExecutor of Java concurrency API.

It has key configuration values which you can configure for your application.

- `corePoolSize`: It is the minimum number of workers which Spring will keep alive without getting timed out.

- `maxPoolSize`: It defines the maximum number of threads that can be created by Spring.

- `queueCapacity`: It defines the capacity for the ThreadPoolExecutor's BlockingQueue.

These configuration values are delegated to underlying Java concurrency API, to the ThreadPoolExecutor.

The major advantage of using ThreadPoolTaskExecutor is that you can modify these values at runtime using **JMX**. But that's the food for another post later on this blog.

## 🎯 Creating ThreadPoolTaskExecutor Configuration
Let's create a ThreadPoolTaskExecutor in a Spring configuration class.
```
@Configuration
@EnableAsync
public class ThreadPoolTaskExecutorConfig {

  @Bean("customThreadPoolTaskExecutor")
  public Executor getThreadPoolTaskExecutor() {
    ThreadPoolTaskExecutor threadPoolTaskExecutor = new ThreadPoolTaskExecutor();
    threadPoolTaskExecutor.setCorePoolSize(2);
    threadPoolTaskExecutor.setMaxPoolSize(2);
    threadPoolTaskExecutor.setQueueCapacity(100);
    threadPoolTaskExecutor.setThreadNamePrefix("utopian-thread-");
    threadPoolTaskExecutor.initialize();
    return threadPoolTaskExecutor;
  }
}
```
You need to annotate `@EnableAsync` on this configuration class. This ensures that Spring uses thread pool in the background to run `@Async` methods in your application.

Now with this configuration created, any asynchronous method you write in your Spring application, will use threads from the thread pool for asynchronous processing.

It becomes a very handy feature, and now you don't need to handle Executors on your own. Spring will do that for you based on the configuration provided.

## 🎯 Creating Asynchronous Method
Let's write a method in service layer, which returns CompletableFuture<List<Employee>>. We want our controller to call this asynchronously. This method returns hard coded list of employees.
```
@Async("customThreadPoolTaskExecutor")
  public CompletableFuture<List<Employee>> getAllEmployees() {

    LOGGER.info("EmployeeService.getAllEmployees() running on: {}",
        Thread.currentThread().getName());

    return CompletableFuture.completedFuture(EMPLOYEES_LIST);
  }
```
I have annotated the method with `@Async` to allow this method to run asynchronously.

Also notice that I have provided the bean name for Executor which I want to use for this method. In case you have multiple Executor defined, you can annotate your methods with `@Async` using the executor bean name that you want to use for that method.

Let's now tie this service method from controller. I want to make a GET call to controller and get the list of employees.

Now since we tied the ThreadPoolTaskExecutor in our application, we expect that threads from our configured thread pool will be picked up and used to process the `@Async` methods.

```
@GetMapping("/employee")
  public ResponseEntity<List<Employee>> getAllEmployees()
      throws ExecutionException, InterruptedException {

    return ResponseEntity.status(HttpStatus.OK).body(employeeService.getAllEmployees().get());
  }
``` 
Time to hit the API now. This API is running at port 8080.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649354822159/-bdfG2I15.png)

Let's hit this URL multiple times. We expect that threads from our thread pool will switch among themselves to process our `@Async` methods in the service layer.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649354983705/gPQva9tRn.png)

As expected! Threads are being picked up from our thread pool and they are being switched among themselves.

Had we two different service hit; say one service hit on `@Async` method1, and another service hit on `@Async` method2 then we will be able to see in the console that both these methods are executing by two different threads concurrently.

## 🎯 Github Repository
I have pushed all the code changes that I used in this article at my [Github](https://github.com/anshulgammy/utopian-nerd/tree/main/usecases/completablefuture-springboot) repository. Please check that out.

It has more than one API; so you will be able test the two asynchronous methods in the service layers being executed by two different threads; when hit at the same time by the client.

I hope this post was useful.