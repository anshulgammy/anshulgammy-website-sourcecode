---
title: "Function Currying in Java"
date: 2022-05-07T14:49:38+05:30
draft: false
categories: [tech, blog]
tags: [functions, functional interfaces, currying, java]
description: "Learn about function currying in Java with a simple use case study on Sandwiches"
summary: "Learn about function currying in Java with a simple use case study on Sandwiches"
author: "Anshul Gautam"
---

## 🎯 Overview
You must be aware of out of the box functional interfaces provided by Java since the advent of version 8.

These functional interfaces like `Function<T, R>`, `BiFunction<T, U, R>`, `Consumer<T>` etc present in `java.util.function` package help you with out of the box functional interfaces so that you need not create them on your own.
Also, they bring uniformity in the code bases by having similar functional interfaces all around for the same type and same usage.

These functional interfaces in one way also have a limitation; on the number of arguments they can handle and work on.

For example, `Function<T, R>` will take in object of type T and apply the function to return object of type R.

What if you want to apply a function based on two objects? Well, yes, there is a `BiFunction<T, U, R>` for you to use. But then, what if you want to have four objects to make use of while applying the function and return some object type?

This problem can be solved using ***currying***.

## 🎯 What is Currying
With currying in Java we can define high order functions and then pass the first order function and function arguments in a chained cascaded fashion.

In other words, we can relate this to having broken a bigger function with many arguments into several functions of single argument, which are chained in such a way that the final output from the original bigger function stays the same.

Let's dive more into this with one problem statement which we will solve using currying in Java.

## 🎯 The problem statement
We have a `Sandwich` class which has properties like bread, veggies, onion, eggs, pepper etc. We want to prepare the Sandwich by instantiating this class.

The `Sandwich` class will be instantiated in `SandwichOvenOperator` class, in a curried Function manner.

## 🎯 Implementation
```
// This class has Builder pattern implemented inside it to create Sandwiches
class Sandwich {

    private String bread;
    private boolean veggies;
    private boolean eggs;
    private boolean meat;
    private boolean pepper;
    private boolean onion;
    .
    .
    .
    .
}
```
There is one `SandwichOven` interface which extends from Function.
```
interface SandwichOven extends
        Function<String,
                Function<Boolean,
                        Function<Boolean,
                                Function<Boolean,
                                        Function<Boolean,
                                                Function<Boolean, Sandwich>>>>>> {

}
```
The class which implements this interface is below:
```
class SandwichOvenOperator implements SandwichOven {

    @Override
    public Function<Boolean,
            Function<Boolean,
                    Function<Boolean,
                            Function<Boolean,
                                    Function<Boolean, Sandwich>>>>> apply(String bread) {
        return veggies -> eggs -> meat -> pepper -> onion -> Sandwich.builder()
                .setBread(bread)
                .setEggs(eggs)
                .setMeat(meat)
                .setOnion(onion)
                .setPepper(pepper)
                .setVeggies(veggies)
                .build();
    }
}
```
Notice how we are returning the `Sandwich` instance using the curried function in the chain.

I am triggering `Sandwich` creation from the `main()` method.

```
public static void main(String[] args) {
    SandwichOven sandwichOven = new SandwichOvenOperator();
    // performing apply() in the order of veggies -> eggs -> meat -> pepper -> onion, which is
    // defined in the SandwichOvenOperator.apply(bread) method.
    Sandwich sandwich = sandwichOven
            .apply("oregano") // for bread
            .apply(true) // for veggies
            .apply(true) // for eggs
            .apply(false) // for meat
            .apply(true) // for pepper
            .apply(false);// for onion
    System.out.println("Sandwich prepared: " + sandwich);
}
```
When we are operating the `SandwichOven` to prepare the Sandwich, we are following the chain order veggies -> eggs -> meat -> pepper -> onion, which is defined in the `SandwichOvenOperator.apply(bread)` method.

## 🎯 Conclusion
I hope this post gave you an overview of currying in Java using a simple example of preparing Sandwiches. All the code changes that I showed in this post here are present at my GitHub [repository](https://github.com/anshulgammy/utopian-nerd/tree/main/usecases/java/java8-function-currying).