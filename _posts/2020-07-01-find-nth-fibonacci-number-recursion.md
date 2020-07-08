---
layout: default
title: "Find The Nth Fibonacci Number In The Sequence Using Recursion"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, fibonacci, recursion]
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

We can solve this problem using recursion by dividing the problem into
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
        int n = Integer.parseInt(args[0]);
        System.out.println(findNthFibonacci(n));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/recursion/Fibonacci.java){:target="_blank" rel="noopener"}

### Notes: 
Time Complexity: T(n-1) + T(n-2) + O(1) = O(2<sup>n</sup>)

Space Complexity: O(n)

Recursion is not a good approach for solving this problem because
the time complexity is exponential. As the value of n grows larger, the
time performance will grow exponentially. You can observe this exponential
growth in runtime by comparing the time it takes when n = 10 versus when
n = 50.

Since the nth Fibonacci number is the sum of the previous two Fibonacci numbers, 
a better approach would be to use dynamic programming when can utilize this fact
to produce a solution with linear time complexity.

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming](/blog/find-nth-fibonacci-number-dynamic-programming.html)

