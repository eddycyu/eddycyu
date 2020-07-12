---
layout: default
title: "Find The Nth Fibonacci Number In The Sequence"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, fibonacci, recursion, dynamic programming]
author: Eddy Yu
published: true
---

This note shows how to find the _nth_ Fibonacci number in the Fibonacci
sequence. We will demonstrate two different approaches to implementing
the solution.

### Problem:
Given a number _n_, where _n_ >= 0, find the _nth_ Fibonacci number in the 
sequence.

### Analysis:
As described on [Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number){:target="_blank" rel="noopener"}, 
Fibonacci numbers form a sequence such that each number is the sum of the two
preceding ones.
```
F0 = 0, F1 = 1, and Fn = Fn-1 + Fn-2 for n > 1
```
For example, the first ten numbers in the sequence are:
```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
``` 
   
### Solution 1 (Recursion):
The first solution uses recursion by dividing the problem into
sub-problems. To solve for F<sub>n</sub>, we need to first solve for
F<sub>n-1</sub> and F<sub>n-2</sub>, and so on.
```java
public class Fibonacci {

    private static long findNthFibonacci(int n) {
        // termination condition
        if (n <= 1) {
            return n;
        }

        // recursive call
        return findNthFibonacci(n - 1) + findNthFibonacci(n - 2);
    }

    public static void main(String[] args) {
        final int n = Integer.parseInt(args[0]);
        System.out.println(findNthFibonacci(n));
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/recursion/Fibonacci.java){:target="_blank" rel="noopener"}

### Solution 1 Notes: 
Time Complexity: T(n-1) + T(n-2) + O(1) = O(2<sup>n</sup>)

Space Complexity: O(n)

Though concise and intuitive, recursion is not a good approach for solving this 
problem because the time complexity is exponential. As the value of _n_ grows 
larger, the time performance will grow exponentially. You can observe this 
exponential growth in runtime by comparing the execution time when _n_ = 10 
versus when _n_ = 50.

Since the _nth_ Fibonacci number is the sum of the previous two Fibonacci 
numbers, a better approach would be to use dynamic programming which can 
utilize this fact to produce a solution with linear time complexity.

### Solution 2 (Dynamic Programming):
Since the _nth_ Fibonacci number is the sum of the previous two Fibonacci 
numbers, we can solve this problem using [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming){:target="_blank" rel="noopener"}, 
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
        final int n = Integer.parseInt(args[0]);
        System.out.println(findNthFibonacci(n));
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/dynamicprogramming/Fibonacci.java){:target="_blank" rel="noopener"}

### Solution 2 Notes: 
Time Complexity: O(n)

Space Complexity: O(1)
