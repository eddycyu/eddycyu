---
layout: default
title: "Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, fibonacci, dynamic programming]
---

### Problem:
Given n, where n >= 0, find the nth Fibonacci number in the sequence.

### Analysis:
As described on [Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number){:target="_blank" rel="noopener"}, 
Fibonacci numbers form a sequence such that each number is the sum of the two
preceding ones.

    F0 = 0, F1 = 1, and Fn = Fn-1 + Fn-2 for n > 1

For example, the first ten numbers in the sequence are:

    0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
    
### Solution:
We know the first two Fibonacci numbers:

    F0 = 0, F1 = 1

We know the formula for the nth Fibonacci number:

    Fn = Fn-1 + Fn-2

Since the nth Fibonacci number is the sum of the previous two Fibonacci numbers, 
we can solve this problem using [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming){:target="_blank" rel="noopener"}, 
which can leverage this fact to produce a solution with linear time complexity.

```java
public class Fibonacci {

    private static long findNthFibonacci(int n) {
        // populate known values
        long fn1 = 0;           // F0 = 0
        long fn2 = 1;           // F1 = 1

        // return known values for n <= 1
        if (n == 0) {
            return fn1;         // F0
        }
        if (n == 1) {
            return fn2;         // F1
        }

        // F2 can be derived since we know F0 and F1
        long fn = fn1 + fn2;    // F2

        // compute Fn for n > 2
        for (int i = 3; i <= n; i++) {
            fn1 = fn2;
            fn2 = fn;
            fn = fn1 + fn2;
        }

        // return values for n > 1
        return fn;
    }

    public static void main(String[] args) {
        int n = Integer.parseInt(args[0]);
        System.out.println(findNthFibonacci(n));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/dynamicprogramming/Fibonacci.java){:target="_blank" rel="noopener"}

### Notes: 
Time Complexity: O(n)

Space Complexity: O(1)

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion.html)

