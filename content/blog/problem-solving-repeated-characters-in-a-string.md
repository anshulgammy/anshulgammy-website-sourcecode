---
title: "Problem 8 - Find Repeated Characters in a String"
date: 2018-02-04T23:23:34+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, repeated-characters, java]
description: "How to find repeated characters in a String using Java"
summary: "How to find repeated characters in a String using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Write a Java program to find repeated characters in a String. For example, if given input to your program is "Java", it should print all duplicates characters along with their count. 

For eg: Java should output repeated character and its count a = 2.

Important concepts to be aware of: 
- This problem can be solved using HashMap. Store character as key in the map, and value as the count of that character.

## 🎯 Solving the problem in Java

```
/**
 * Problem 8: Write a Java program to find repeated characters in a String. For example, if given
 * input to your program is "Java", it should print all duplicates characters along with their
 * count. eg: Java should output repeated character and its count a = 2.
 */
public class Problem_8 {

  public static void main(String[] args) {

    System.out.println("Enter the string:");
    Scanner sc = new Scanner(System.in);

    String inputString = sc.next();

    calculateRepeatedCharactersUsingHashMap(inputString);
  }

  private static void calculateRepeatedCharactersUsingHashMap(String inputString) {

    Map<Character, Integer> characterCountMap = new HashMap<>();

    for (int i = 0; i < inputString.length(); i++) {
      if (characterCountMap.containsKey(inputString.charAt(i))) {
        characterCountMap.put(inputString.charAt(i),
            characterCountMap.get(inputString.charAt(i)) + 1);
      } else {
        characterCountMap.put(inputString.charAt(i), 1);
      }
    }

    characterCountMap.entrySet().stream().filter(entry -> entry.getValue() > 1)
        .forEach(entry -> System.out.println(entry.getKey() + " : " + entry.getValue()));
  }
}
```

**Output**:
```
Java
a : 2

Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_8.java).
