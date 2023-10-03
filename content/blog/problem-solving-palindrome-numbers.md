---
title: "Problem 3 - Palindrome String"
date: 2018-02-03T23:20:18+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, palindrome-string, java]
description: "How to check if a given string is palindrome or not using Java"
summary: "How to check if a given string is palindrome or not using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
A string can be called as palindrome if it is equal to its reversed string.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java program to check if a given String is palindrome or not.

```
/**
 * Problem 3: Write a Java program to check if a given String is palindrome or not.
 */
public class Problem_3 {

  public static void main(String[] args) {

    System.out.println("Enter the string:");
    Scanner sc = new Scanner(System.in);

    String str = sc.next();

    if (str == null || str.trim().length() == 0) {
      System.out.println("Not Palindrome");
    }

    // naiveWay(str);

    if (betterWay(str)) {
      System.out.println("Palindrome");
    } else {
      System.out.println("Not Palindrome");
    }
  }

  private static boolean betterWay(String str) {

    int midPosition = str.length() / 2;

    for (int i = 0; i < midPosition; i++) {
      if (str.charAt(i) != str.charAt(str.length() - 1 - i)) {
        return false;
      }
    }

    return true;
  }

  private static void naiveWay(String str) {
    String reversedString = "";

    for (int i = str.length() - 1; i >= 0; i--) {
      reversedString = reversedString + str.charAt(i);
    }

    if (reversedString.equals(str)) {
      System.out.println("Palindrome");
    } else {
      System.out.println("Not Palindrome");
    }
  }
}
```

**Output**:
```
Enter the string:
abcd
Not Palindrome

Process finished with exit code 0

```

```
Enter the string:
aabaa
Palindrome

Process finished with exit code 0
```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_3.java).
