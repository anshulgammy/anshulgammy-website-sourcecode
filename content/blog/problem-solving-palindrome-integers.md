---
title: "Problem 4 - Palindrome Integers"
date: 2018-02-03T23:20:33+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, palindrome-integers, java]
description: "How to check if a given integer is palindrome or not using Java"
summary: "How to check if a given integer is palindrome or not using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
An integer can be called as a Palindrome when that integer is equal to the integer formed by it's digits in reversed order. For eg, 121 is a palindrome integer.

Important concepts to be aware of: 
- For a number N, we can get hold of its digits in form of remainders by dividing by 10. Keep dividing the number (`number = number / 10`) until the quotient is 0.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java program to check if a given Integer is palindrome or not.

```
/**
 * Problem 4: Write a Java program to check if a given Integer is palindrome or not.
 */
public class Problem_4 {

  public static void main(String[] args) {

    System.out.println("Enter the number:");
    Scanner sc = new Scanner(System.in);

    int targetNumber = sc.nextInt();
    int number = targetNumber;
    int reversedNumber = 0;

    while (number > 0) {
      int remainder = number % 10;
      number = number / 10;
      reversedNumber = (reversedNumber * 10) + remainder;
    }

    if (reversedNumber == targetNumber) {
      System.out.println("Palindrome");
    } else {
      System.out.println("Not Palindrome");
    }
  }
}
```

**Output**:
```
Enter the number:
121
Palindrome

Process finished with exit code 0

```

```
Enter the number:
345
Not Palindrome

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_4.java).
