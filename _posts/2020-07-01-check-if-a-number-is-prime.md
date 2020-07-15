---
layout: default
title: "Check If A Number Is Prime"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, prime]
author: Eddy Yu
published: true
---

This recipe shows how to check if a given number is prime. We will
demonstrate two different approaches to implementing the solution.

### Problem:
Given a number, check if it is a prime number.

### Analysis:
As described on [Wikipedia](https://en.wikipedia.org/wiki/Prime_number){:target="_blank" rel="noopener"}, 
a prime number is a natural number greater than 1 that is not a product of two 
smaller natural numbers.

For example, the first few prime numbers are:
```
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53
```

### Solution 1:
The first solution is fast and uses the following facts:
* for numbers <= 3, only 2 and 3 are prime
* all primes greater than 3 (e.g. 5, 7, 11, 13...) are of the form 6k&plusmn;1
* eliminate numbers that are divisible by 2 or 3
* only check for divisors up to sqrt(number)
 
[Wikipedia](https://en.wikipedia.org/wiki/Primality_test){:target="_blank" rel="noopener"}
describes this algorithm in more detail.

```java
public class Prime1 {

    private static boolean isPrime(int number) {
        // special case: for numbers 3 and under, only 2 and 3 are prime
        if (number <= 3) {
            return number > 1;
        }
        // all primes greater than 3 are of the form 6k[+/-]1;
        // eliminate numbers divisible by 2 or 3
        if ((number % 2 == 0) || (number % 3 == 0)) {
            return false;
        }
        // and only check for divisors up to sqrt(number)
        for (int i = 5; i * i <= number; i += 6) {
            if ((number % i == 0) || ((number % (i + 2)) == 0)) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        final int number = Integer.parseInt(args[0]);
        System.out.println(isPrime(number));
    }
}

``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/prime/Prime1.java){:target="_blank" rel="noopener"}

### Solution 2:
The next solution is simpler but less optimized. It uses the following facts:
 * for numbers <= 2, only 2 is prime
 * even numbers are not prime
 * only check odd numbers for prime
 * only check for divisors up to sqrt(number)
 
```java
public class Prime2 {

    private static boolean isPrime(int number) {
        // special case: for numbers 2 and under, only 2 is prime
        if (number <= 2) {
            return number > 1;
        }
        // eliminate even numbers which are not prime
        if (number % 2 == 0) {
            return false;
        }
        // only check odd numbers for prime, and
        // only check for divisors up to sqrt(number)
        for (int i = 3; i * i <= number; i += 2) {
            if (number % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        final int number = Integer.parseInt(args[0]);
        System.out.println(isPrime(number));
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/prime/Prime2.java){:target="_blank" rel="noopener"}
