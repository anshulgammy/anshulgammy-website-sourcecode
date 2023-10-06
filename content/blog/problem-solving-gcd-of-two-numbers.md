---
title: "Problem 9 - GCD of Two Numbers"
date: 2018-02-04T23:33:34+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, gcd, java]
description: "How to find GCD of two numbers using Java"
summary: "How to find GCD of two numbers using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Write a Java program to find the Greatest Common Divisor(GCD) of two numbers.

Important concepts to be aware of: 
- This problem can be solved using Euclidean algorithm by subtraction. This algo says that GCD of two numbers = GCD of difference of the two and the smaller number.
- This problem can also be solved if we keep dividing the bigger number by smaller number, until the remainder is zero. This is a better approach to solve this problem.

I have solved using both approaches below.

## 🎯 Solving the problem in Java

```
/**
 * Problem 9: Write a Java program to find the Greatest Common Divisor(GCD) of two numbers.
 */
public class Problem_9 {

  public static void main(String[] args) {

    System.out.println("Enter the first number:");
    Scanner sc = new Scanner(System.in);

    int firstNumber = sc.nextInt();

    System.out.println("Enter the second number:");

    int secondNumber = sc.nextInt();

    System.out.println("GCD is: " + calculateGCD(firstNumber, secondNumber));
    System.out.println("GCD is: " + calculateGCDBetterWay(firstNumber, secondNumber));
  }

  // Also known as Euclidean algorithm by subtraction.
  // This algo says that GCD of two numbers = GCD of difference of the two and the smaller number.
  private static int calculateGCD(int firstNumber, int secondNumber) {

    if (firstNumber == 0) {
      return secondNumber;

    } else if (secondNumber == 0) {
      return firstNumber;

    } else if (firstNumber == secondNumber) {
      return firstNumber;
    }

    if (firstNumber > secondNumber) {
      return calculateGCD(firstNumber - secondNumber, secondNumber);

    } else {
      return calculateGCD(secondNumber - firstNumber, firstNumber);
    }
  }

  // Keep dividing the bigger number by smaller number, until the remainder is zero.
  private static int calculateGCDBetterWay(int firstNumber, int secondNumber) {
    if(firstNumber == 0) {
      return secondNumber;
    }
    return calculateGCDBetterWay(secondNumber % firstNumber, firstNumber);
  }
}
```

**Output**:
```
Enter the first number:
23
Enter the second number:
21
GCD is: 1
GCD is: 1

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_9.java).
