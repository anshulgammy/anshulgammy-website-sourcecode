---
title: "Problem 6 - Factorial of a Number"
date: 2018-02-04T23:21:33+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, factorial, java]
description: "How to calculate factorial of a given number using Java"
summary: "How to calculate factorial of a given number using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Factorial of a number is multiplication of that number by every number below it till 1.

For eg:
`4! = 4 x 3 x 2 x 1`

Important concepts to be aware of: 
- Get of hold of factorial of 1 as 1, and use that in your logic forward in the program.
- This problem can be solved using iterative approach, as well as using recursive approach. I attempted both these approaches in the code shared below.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java program to calculate factorial of a given number.

```
/**
 * Problem 6: Write a Java program to calculate factorial of a given number.
 */
public class Problem_6 {

  public static void main(String[] args) {

    System.out.println("Enter the number:");
    Scanner sc = new Scanner(System.in);

    int number = sc.nextInt();

    if(number < 0) {
      System.out.println("Factorial cannot be calculated for a negative number");
      System.exit(0);
    }

    System.out.println(String.format("Factorial is %s", calculateFactorialIterative(number)));
    System.out.println(String.format("Factorial is %s", calculateFactorialRecursive(number)));
    System.out.println(String.format("Factorial is %s", calculateFactorialTernary(number)));
  }

  private static int calculateFactorialTernary(int number) {
    return number == 0 ? 1 : number * calculateFactorialTernary(--number);
  }

  private static int calculateFactorialRecursive(int number) {

    if(number == 0) {
      return 1;
    }

    return number * calculateFactorialRecursive(--number);
  }

  private static int calculateFactorialIterative(int number) {
    int factorial = 1;

    for (int i = 2; i <= number; i++) {
      factorial = factorial * i;
    }

    return factorial;
  }
}
```

**Output**:
```
Enter the number:
4
Factorial is 24
Factorial is 24
Factorial is 24

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_6.java).
