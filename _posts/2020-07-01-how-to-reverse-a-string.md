---
layout: default
title: "How To Reverse A String"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, recursion, iteration, reverse string]
author: Eddy Yu
published: true
---

This recipe shows how to reverse a string. We will explore three different
approaches to implementing the solution.

### Problem:
Given an input string, output the reverse string. For example, for the 
input "life is good", the reverse output would be "doog si efil".

### Solution 1:
The easiest solution is to utilize the _StringBuilder_ class provided in Java.
```java
public class ReverseString {

    private static String reverse(String input) {
        final StringBuilder builder = new StringBuilder(input);
        return builder.reverse().toString();
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(reverse(input));
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/string/ReverseString.java){:target="_blank" rel="noopener"}

### Solution 2:
The next solution demonstrates an implementation using iteration.
```java
public class ReverseString {

    private static String reverse(String input) {
        String reverse = "";
        for (int i = input.length() - 1; i >= 0; i--) {
            reverse += input.charAt(i);
        }
        return reverse;
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(reverse(input));
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/iteration/ReverseString.java){:target="_blank" rel="noopener"}

### Solution 3:
The final solution demonstrates an implementation using recursion.
```java
public class ReverseString {

    private static String reverse(String input) {
        if ((input == null) || input.isEmpty()) {
            return input;
        }
        return reverse(input.substring(1)) + input.charAt(0);
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(reverse(input));
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/recursion/ReverseString.java){:target="_blank" rel="noopener"}

### Notes: 

### Links:
* [Check If A String Is A Palindrome](/blog/check-if-a-string-is-a-palindrome)

