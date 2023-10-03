---
title: "Problem 2 - Prime Numbers"
date: 2018-02-03T23:19:18+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, prime-number, java]
description: "How to check if a given number is prime or not using Java"
summary: "How to check if a given number is prime or not using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Prime numbers are numbers that are greater than 1 which have only have two factors: 1 and the number itself.

Important concepts to be aware of: 
- For a number N, it can only be divisible completely by N/2. For numbers greater than N/2, it cannot divide N completely.
- For a number N, it will have divisors(if completely divisible) which will be less than or equal to square root of N.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java program to check if a given number is prime or not.

```
/**
 * Problem 2: Write a Java program to check if a given number is prime or not
 */
public class Problem_2 {

  public static void main(String[] args) {

    System.out.println("Enter the value for n:");
    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();

    if (n <= 1) {
      System.out.println("Not Prime");
      System.exit(0);
    }

    for (int i = 2; i <= n / 2; i++) {
      if (n % i == 0) {
        System.out.println("Not Prime");
        System.exit(0);
      }
    }

    System.out.println("Prime");
  }
}
```

**Output**:
```
Enter the value for n:
33
Not Prime

Process finished with exit code 0
```

```
Enter the value for n:
11
Prime

Process finished with exit code 0
```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_2.java).
