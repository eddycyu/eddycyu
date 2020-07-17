---
layout: default
title: "Check If A String Is A Palindrome"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, palindrome, recursion, iterator]
author: Eddy Yu
published: true
---

This recipe shows how to check if a given string is a palindrome. We will
demonstrate three different approaches to implementing the solution.

### Problem:
Given an input string, check if it is a palindrome.

### Analysis:
As described on [Wikipedia](https://en.wikipedia.org/wiki/Palindrome){:target="_blank" rel="noopener"}, 
a palindrome is a word, number, phrase, or other sequence of characters which 
reads the same backward as forward.

For example, the following are palindromes:
```
radar
kayak
12321
```
To check if an input is a palindrome, the easiest approach would be to first 
reverse the input and them compare the reversed input with the original input. 
The input is a palindrome if they are the same.

### Solution 1:
The first solution uses the _StringBuilder_ class to make things a bit easier.
```java
public class Palindrome1 {

    private static String reverse(String input) {
        final StringBuilder builder = new StringBuilder(input);
        return builder.reverse().toString();
    }

    private static boolean isPalindrome(String input) {
        return (input.equals(reverse(input)));
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(isPalindrome(input));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/palindrome/Palindrome1.java){:target="_blank" rel="noopener"}

### Solution 2:
The next solution demonstrates an implementation using iteration.
```java
public class Palindrome2 {

    private static String reverse(String input) {
        String reverse = "";
        for (int i = input.length() - 1; i >= 0; i--) {
            reverse += input.charAt(i);
        }
        return reverse;
    }

    private static boolean isPalindrome(String input) {
        return (input.equals(reverse(input)));
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(isPalindrome(input));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/palindrome/Palindrome2.java){:target="_blank" rel="noopener"}

### Solution 3:
The final solution demonstrates an implementation using recursion.
```java
public class Palindrome3 {

    private static String reverse(String input) {
        if ((input == null) || input.isEmpty()) {
            return input;
        }
        return reverse(input.substring(1)) + input.charAt(0);
    }

    private static boolean isPalindrome(String input) {
        return (input.equals(reverse(input)));
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(isPalindrome(input));
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/palindrome/Palindrome3.java){:target="_blank" rel="noopener"}

### Links:
* [How To Reverse A String](/blog/how-to-reverse-a-string)
