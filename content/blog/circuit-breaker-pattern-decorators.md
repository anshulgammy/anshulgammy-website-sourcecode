---
title: "Circuit Breaker Pattern using Resilience4j Decorators"
date: 2022-05-05T14:49:38+05:30
draft: false
categories: [tech, blog]
tags: [circuit breaker, resilience4j, decorators, java]
description: "Learn about Resilience4j with simpler examples for retry and circuit breaker design pattern"
summary: "Learn about Resilience4j with simpler examples for retry and circuit breaker design pattern"
author: "Anshul Gautam"
---

This post is in continuation of my [previous](/blog/circuit-breaker-pattern-annotations) post on this blog. In the previous post I discussed on what is circuit breaker, and how we can implement that using Resilience4j annotations.

In this post I will be discussing on implementing the same use case with Resilience4j, but this time using ***Decorators***.

In case you are coming to this post directly, I will urge you to read through my [previous](/blog/circuit-breaker-pattern-annotations) post first on Resilience4j.

## 🎯 Recap of the use case from previous post
The caller method will make call to another method which will throw RuntimeException. Our circuit breaker implementation will keep an eye on this method throwing exception, and in case there is any exception it will provide a fallback route which will get executed when there is an exception.

This is a very simple use case. But it gives you an idea how to bind the circuit breaker implementation to guard your methods and provide a fallback route. 

All this will be done in this post using decorators offered by Resilience4j. Remember that we need to have minimum Java version 8. This is because Resilience4j decorators are built on top of Java 8 functional interfaces.

## 🎯 Integrate Resilience4j in Spring Boot Project
We will be adding below dependencies in pom.xml. Notice that we are not adding Spring AOP dependency here like we had to include when we were implementing using annotations.
```
<!-- Below dependency gives Retry support in Resilience4j -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-retry</artifactId>
    <version>${resilience4j-version}</version>
</dependency>

<!-- Below dependency gives CircuitBreaker support in Resilience4j -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-circuitbreaker</artifactId>
    <version>${resilience4j-version}</version>
</dependency>

<!-- Below dependency provides core circuit breaker support in Resilience4j -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-core</artifactId>
    <version>${resilience4j-version}</version>
</dependency>

<!-- Below dependency provides decorator support/functional interface in Resilience4j for Java 8 and above -->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-all</artifactId>
    <version>${resilience4j-version}</version>
</dependency>

```
## 🎯 Implementation using Decorators
Resilience4j make use of Java 8 functional interfaces for its decorators. 
The below method is explicitly throwing RuntimeException.
```
/**
 * We have configured Retry and CircuitBreaker on this method.
 * Idea is to try 'Retry' first for the configured number of times. If even after retrying this for the
 * configured number of times we continue to get exception, then CircuitBreaker will come into action.
 */
public void runService() {
    Decorators.ofSupplier(() -> {
                LOGGER.info("Printing from ResilientService.runService() for Decorator Implementation");
                if (true) {
                    throw new RuntimeException("Some RuntimeException occurred!");
                }
                return null;
            }).withRetry(retryService)
            .withCircuitBreaker(circuitBreaker)
            .withFallback(Arrays.asList(RuntimeException.class),
                    e -> {
                        fallbackService();
                        return null;
                    }).
            decorate().get();
}
```
In the method above, we have plugged in four independent components. Let's see one by one each one of them.

1. Actual logic which the method is supposed to do. This is the code instruction which can throw exception, and hamper the flow in our application. This code instruction has been supplied in `Decorators.ofSupplier()`. Circuit Breaker will trip when exception is thrown from here.
2. `Retry` component. This is the component which has retry related configuration within it using which retry will be performed. This is supplied using `.withRetry()`.
3. `CircuitBreaker` component. This is the component which has circuit breaking logic instruction within it using resilience4j configuration. This is supplied using `.withCircuitBreaker()`.
4. `Fallback` component. This is the component which will be executed as part of fallback, when there is an exception of the provided type. In the example, I have plugged in `fallbackService()` method which will be executed when there is a `RuntimeException` raised. This is supplied within `.withFallback()`.

Now, let's see how I am preparing Retry and CircuitBreaker components.

## 🎯 Preparing the Retry component for Resilience4j decorator
```
@Bean
@Qualifier(Constants.RESILIENCE_RETRY_NAME)
public Retry resilientServiceRetry(
        @Value("${resilience4j.retry.name}") String retryName,
        @Value("${resilience4j.retry.retryService.maxRetryAttempts}") Integer maxAttempts,
        @Value("${resilience4j.retry.retryService.waitDuration}") Integer waitDurationSeconds
) {
    final RetryConfig config = RetryConfig.custom()
            .maxAttempts(maxAttempts)
            .retryExceptions(RuntimeException.class)
            .waitDuration(Duration.ofSeconds(waitDurationSeconds))
            .build();
    final RetryRegistry registry = RetryRegistry.of(config);
    Retry retry = registry.retry(retryName, config);
    return retry;
}
```
Configuration key value pairs are present in application.properties file. I am injecting them in the method and then preparing the Retry object.

`maxAttempts` define the number of times retry operation will be performed.

In `retryExceptions` I have provided the exception class on which retry and circuit breaker will action. This method takes in `@Nullable Class<? extends Throwable>... errorClasses`.

`waitDuration` is used to specify the wait duration after which each retry will be performed.

## 🎯 Preparing the CircuitBreaker component for Resilience4j decorator
```
@Bean
@Qualifier(Constants.RESILIENCE_CIRCUIT_BREAKER_NAME)
public CircuitBreaker resilientServiceCircuitBreaker(
        @Value("${resilience4j.circuitbreaker.name}") String circuitBreakerName,
        @Value("${resilience4j.circuitbreaker.retryServiceCircuitBreaker.waitDurationInOpenState}") Integer waitDurationSeconds,
        @Value("${resilience4j.circuitbreaker.retryServiceCircuitBreaker.slidingWindowSize}") Integer slidingWindowSize,
        @Value("${resilience4j.circuitbreaker.retryServiceCircuitBreaker.failureRateThreshold}") Float failureRateThreshold,
        @Value("${resilience4j.circuitbreaker.retryServiceCircuitBreaker.permittedNumberOfCallsInHalfOpenState}") Integer permittedNumberOfCallsInHalfOpenState
) {
    final CircuitBreakerConfig config = CircuitBreakerConfig
            .custom()
            .slidingWindowType(CircuitBreakerConfig.SlidingWindowType.COUNT_BASED)
            .slidingWindowSize(slidingWindowSize)
            .failureRateThreshold(failureRateThreshold)
            .waitDurationInOpenState(Duration.ofSeconds(waitDurationSeconds))
            .permittedNumberOfCallsInHalfOpenState(permittedNumberOfCallsInHalfOpenState)
            .build();
    final CircuitBreakerRegistry registry = CircuitBreakerRegistry.of(config);
    final CircuitBreaker circuitBreaker = registry.circuitBreaker(circuitBreakerName);
    return circuitBreaker;
}
```
For creating circuit breaker component I am again fetching the configuration key values from application properties file and injecting them in the method. These values are used to create the `CircuitBreakerRegistry` object, from which we get the `CircuitBreaker` component.

## 🎯 Configuration properties
Configuration key value pairs are same as what we used while implementing using resilience4j annotations.
```
#Resilience4j Retry specific configuration goes below
resilience4j.retry.name=retryService
#resilience4j.retry.retryAspectOrder=2
resilience4j.retry.retryService.maxRetryAttempts=3
resilience4j.retry.retryService.waitDuration=1

#Resilience4j CircuitBreaker specific configuration goes below
resilience4j.circuitbreaker.name=retryServiceCircuitBreaker
#resilience4j.circuitbreaker.circuitBreakerAspectOrder=1
resilience4j.circuitbreaker.retryServiceCircuitBreaker.waitDurationInOpenState=3
resilience4j.circuitbreaker.retryServiceCircuitBreaker.permittedNumberOfCallsInHalfOpenState=3
#resilience4j.circuitbreaker.retryServiceCircuitBreaker.slidingWindowType=COUNT_BASED
resilience4j.circuitbreaker.retryServiceCircuitBreaker.slidingWindowSize=2
resilience4j.circuitbreaker.retryServiceCircuitBreaker.failureRateThreshold=30
```
## 🎯 Conclusion
In this post, I used resilience4j implementation using decorators. I hope this was helpful for you. All the code changes that I shown in this post are present in my GitHub [repository](https://github.com/anshulgammy/utopian-nerd/tree/main/usecases/resilience4j-with-springboot/resilience4j-decorator).