---
title: "Problem 7 - Remove Duplicate Elements in Array"
date: 2018-02-04T23:22:34+05:30
draft: false
categories: [tech, blog]
tags: [problem-solving, duplicate-elements, java]
description: "How to remove duplicate elements from the array using Java"
summary: "How to remove duplicate elements from the array using Java"
author: "Anshul Gautam"
---

## 🎯 Overview
Given an array of numbers, remove duplicate elements from this array.

Important concepts to be aware of: 
- This problem can be solved using HashSet in Java. Using Set allows us to store unique elements. So we can add all the elements of the array is a Set, and this way our set will have unique elements only.
- This problem can also be solved using String. Store elements as a string, and utilize string.contains(element) to know if already present or not. But this is not a recommended way to solve this problem.
- This problem can also be solved using **Frequency Array**. Frequency Array is a concept which is very commonly used in different problems.
- In frequency array, we first find out the largest element in the array. Then, we create a new array `frequencyArray` of size `largestNumber + 1`. Then:
```
for (int i = 0; i < originalNumberArray.length; i++) {
  frequencyArray[originalNumberArray[i]]++;
}
```
Then in frequencyArray whatever index yields a number > 0, just print that index out. Wherever we have duplicate elements, at those indexes we will have number > 1. All those indexes where we have number == 1, those indexes are unique.
- This problem can also be solved using sorting. Just sort the input array in ascending order. Then using a reference variable = -1, interate and compare if the numbers are getting repeated in the array or not.

I attempted to solve this problem using all these methods below.

## 🎯 Solving the problem in Java

**Problem Statement**: Write a Java Program to Remove Duplicate Elements From the Array.

```
/**
 * Problem 7: Write a Java Program to Remove Duplicate Elements From the Array.
 */
public class Problem_7 {

  public static void main(String[] args) {

    System.out.println("Enter the elements, separated by comma:");
    Scanner sc = new Scanner(System.in);

    String commaSeparatedString = sc.nextLine();

    usingString(commaSeparatedString);
    usingFrequencyArray(commaSeparatedString);
    usingSorting(commaSeparatedString);
    // using HashSet
  }

  private static void usingSorting(String commaSeparatedString) {
    String[] originalNumberStringArray = commaSeparatedString.split(",");
    Arrays.sort(originalNumberStringArray);

    int[] originalNumberArray = new int[originalNumberStringArray.length];

    for (int i = 0; i < originalNumberStringArray.length; i++) {
      originalNumberArray[i] = Integer.parseInt(originalNumberStringArray[i]);
    }

    int prevNumber = -1;

    for (int i = 0; i < originalNumberArray.length; i++) {
      if (prevNumber != originalNumberArray[i]) {
        System.out.print(originalNumberArray[i] + " ");
        prevNumber = originalNumberArray[i];
      }
    }
  }

  private static void usingFrequencyArray(String commaSeparatedString) {
    String[] originalNumberStringArray = commaSeparatedString.split(",");
    int[] originalNumberArray = new int[originalNumberStringArray.length];

    for (int i = 0; i < originalNumberStringArray.length; i++) {
      originalNumberArray[i] = Integer.parseInt(originalNumberStringArray[i]);
    }

    int largestNumber = 0;

    for (int i = 0; i < originalNumberArray.length; i++) {
      if (originalNumberArray[i] > largestNumber) {
        largestNumber = originalNumberArray[i];
      }
    }

    int[] frequencyArray = new int[largestNumber + 1];

    for (int i = 0; i < originalNumberArray.length; i++) {
      frequencyArray[originalNumberArray[i]]++;
    }

    for (int i = 0; i < frequencyArray.length; i++) {
      if (frequencyArray[i] > 0) {
        System.out.print(i + " ");
      }
    }

    System.out.println();
  }

  private static void usingString(String commaSeparatedString) {
    String[] originalNumberArray = commaSeparatedString.split(",");

    String uniqueNumberArrayString = "";

    for (int i = 0; i < originalNumberArray.length; i++) {
      if (!uniqueNumberArrayString.contains(originalNumberArray[i])) {
        if (i != 0) {
          uniqueNumberArrayString += ",";
          uniqueNumberArrayString += originalNumberArray[i];
        } else {
          uniqueNumberArrayString += originalNumberArray[i];
        }
      }
    }

    System.out.println("Unique numbers are: " + uniqueNumberArrayString);
  }
}
```

**Output**:
```
Enter the elements, separated by comma:
1,2,3,3,2,1,4,5,3
Unique numbers are: 1,2,3,4,5
1 2 3 4 5 
1 2 3 4 5 
Process finished with exit code 0

```

## 🎯 Github
Code shared in this post can be found [here](https://github.com/anshulgammy/problem-solving-with-java/blob/main/src/com/utopian/nerd/problem/solving/Problem_7.java).
