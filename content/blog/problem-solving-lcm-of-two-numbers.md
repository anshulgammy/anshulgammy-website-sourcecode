---
title: "Problem 10 - LCM of Two Numbers"
date: 2018-02-04T23:35:34+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, lcm, java]
description: "How to find LCM of two numbers using Java"
summary: "How to find LCM of two numbers using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Write a Java program to find the Lowest Common Multiple(LCM) of two numbers.

Important concepts to be aware of: 
- product of two numbers = their gcd x their lcm

## 🎯 Solving the problem in Java

```
/**
 * Problem 10: Write a Java program to find the Lowest Common Multiple(LCM) of two numbers.
 */
public class Problem_10 {

  public static void main(String[] args) {

    Scanner sc = new Scanner(System.in);

    System.out.println("Enter the first number:");
    int firstNumber = sc.nextInt();

    System.out.println("Enter the second number:");
    int secondNumber = sc.nextInt();

    System.out.println("LCM is: " + calculateLCM(firstNumber, secondNumber));
  }

  private static int calculateLCM(int firstNumber, int secondNumber) {
    int lcm = (firstNumber * secondNumber) / calculateGCD(firstNumber, secondNumber);
    return lcm;
  }

  private static int calculateGCD(int firstNumber, int secondNumber) {
    if(firstNumber == 0) {
      return secondNumber;
    }
    return calculateGCD(secondNumber % firstNumber, firstNumber);
  }
}
```

**Output**:
```
Enter the first number:
12
Enter the second number:
24
LCM is: 24

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_10.java).
