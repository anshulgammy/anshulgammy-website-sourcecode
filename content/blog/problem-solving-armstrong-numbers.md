---
title: "Problem 5 - Armstrong Numbers"
date: 2018-02-03T23:21:33+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, armstrong-numbers, java]
description: "How to check to check if a given Integer is Armstrong number or not. using Java"
summary: "How to check to check if a given Integer is Armstrong number or not. using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
A number can be called as an Armstrong number if the sum of it's own digits raised to the power number of number of digits gives the number itself.

Important concepts to be aware of: 
- For a number N, we can get hold of its digits in form of remainders by dividing by 10. Keep dividing the number (`number = number / 10`) until the quotient is 0.
- Java's Math.pow(base, power) will return double. For integers, we need to type cast.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java program to check if a given Integer is Armstrong number or not.

```
/**
 * Problem 5: Write a Java program to check if a given Integer is Armstrong number or not.
 */
public class Problem_5 {

  public static void main(String[] args) {

    System.out.println("Enter the number:");
    Scanner sc = new Scanner(System.in);

    int targetNumber = sc.nextInt();
    int order = String.valueOf(targetNumber).length();

    int number = targetNumber;
    int result = 0;

    while (number > 0) {
      result = (int) (result + Math.pow(number % 10, order));
      number = number / 10;
    }

    if (targetNumber == result) {
      System.out.println("Armstrong Number");
    } else {
      System.out.println("Not Armstrong Number");
    }
  }
}
```

**Output**:
```
Enter the number:
1634
Armstrong Number

Process finished with exit code 0

```

```
Enter the number:
123
Not Armstrong Number

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_5.java).
