---
title: "Problem 11 - Prime Factors of a Number"
date: 2018-02-04T23:45:34+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, prime-factors, java]
description: "How to find prime factors of a number using Java"
summary: "How to find prime factors of a number using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Write a Java program to list all the prime factors of a number.

## 🎯 Solving the problem in Java

```
/**
 * Problem 11: Write a Java program to list all the prime factors of a number.
 */
public class Problem_11 {

  public static void main(String[] args) {

    Scanner sc = new Scanner(System.in);

    System.out.println("Enter the number:");
    int number = sc.nextInt();

    System.out.println("Prime factors are:");
    findPrimeFactors(number);
  }

  private static void findPrimeFactors(int number) {

    // also we can use i <= Math.sqrt(number);
    for (int i = 2; i <= number / 2; i++) {
      while (number % i == 0) {
        number = number / i;
        System.out.print(i + " ");
      }
    }

    if (number > 1) {
      System.out.print(number);
    }
  }
}
```

**Output**:
```
Enter the number:
24
Prime factors are:
2 2 2 3
Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_11.java).
