---
layout: default
title: "Check If A String Is A Palindrome"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, palindrome]
---

### Problem:
Given a string input, check if it is a palindrome.

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

### Solution:
The first solution uses the Java _StringBuilder_ class to make things a bit easier.
```java
public class Palindrome1 {

    private static boolean isPalindrome(String input) {
        final StringBuilder buffer = new StringBuilder(input);
        return (buffer.reverse().toString().equals(input));
    }

    public static void main(String[] args) {
        String input = args[0];
        System.out.println(isPalindrome(input));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/palindrome/Palindrome1.java){:target="_blank" rel="noopener"}

The next solution is implemented without using _StringBuilder_.
```java
public class Palindrome2 {

    private static boolean isPalindrome(String input) {
        String reverse = "";
        for (int i = input.length() - 1; i >= 0; i--) {
            reverse += input.charAt(i);
        }
        return (reverse.equals(input));
    }

    public static void main(String[] args) {
        String input = args[0];
        System.out.println(isPalindrome(input));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/palindrome/Palindrome2.java){:target="_blank" rel="noopener"}

### Links:
* [Dual Palindromes](/usaco/dualpal)
* [Palindromic Squares](/usaco/palsquare)

