---
layout: default
title: "Superprime Rib"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, recursion, prime, palindrome]
order: 1.6.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Butchering Farmer John's cows always yields the best prime rib. You can tell 
prime ribs by looking at the digits lovingly stamped across them, one by one, 
by FJ and the USDA. Farmer John ensures that a purchaser of his prime ribs 
gets really prime ribs because when sliced from the right, the numbers on the 
ribs continue to stay prime right down to the last rib, e.g.:
```
7  3  3  1
```
The set of ribs denoted by 7331 is prime; the three ribs 733 are prime; the 
two ribs 73 are prime, and, of course, the last rib, 7, is prime. The number 
7331 is called a superprime of length 4.

Write a program that accepts a number N 1 <=N<=8 of ribs and prints all the 
superprimes of that length.

The number 1 (by itself) is not a prime number.

##### PROGRAM NAME: sprime

##### INPUT FORMAT
A single line with the number N.

##### SAMPLE INPUT (file sprime.in)
```
4
```

##### OUTPUT FORMAT
The superprime ribs of length N, printed in ascending order one per line.

##### SAMPLE OUTPUT (file sprime.out)
```
2333
2339
2393
2399
2939
3119
3137
3733
3739
3793
3797
5939
7193
7331
7333
7393
```

### Analysis:
This problem can be solved using recursion.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Arrays;

public class sprime {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("sprime.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("sprime.out")))) {

            final int N = Integer.parseInt(f.readLine());

            // find min and max value ranges
            int min = 1;
            for (int n = 1; n < N; n++) {
                min = min * 10;
            }
            int max = min * 10;
            max -= 1;

            // note1: use 'char' instead of 'int' to reduce memory usage
            // note2: only store computed values up to (max/10) to reduce memory usage
            final char[] sprimes = new char[max / 10];

            // initialize default values to '2' where '0' = not super prime,
            // '1' = super prime, and '2' = don't know if super prime or not
            Arrays.fill(sprimes, '2');

            // find all superprimes of length N
            for (int i = min + 1; i <= max; i += 2) {
                if (isSuperPrime(i, sprimes)) {
                    out.println(i);
                }
            }
        }
    }

    private static boolean isSuperPrime(int number, char[] sprimes) {
        if (number >= 10) {
            int nextNumber = number / 10;

            // optimization: check if slice is superprime (first check against known superprimes)
            if (sprimes[nextNumber - 1] != '2') {
                return sprimes[nextNumber - 1] == '1' && isPrime(number);
            }

            // otherwise check if slice is superprime by computing
            final boolean result = isSuperPrime(nextNumber, sprimes);
            if (sprimes[nextNumber - 1] == '2') {
                sprimes[nextNumber - 1] = result ? '1' : '0';
            } else {
                sprimes[nextNumber - 1] = (result && (sprimes[nextNumber - 1] == '1')) ? '1' : '0';
            }
            return result && isPrime(number);
        } else {
            // no more slices; just check if number is prime
            return isPrime(number);
        }
    }

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
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/pprime.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion)
* [Check If A Number Is Prime](/blog/check-if-a-number-is-prime)
* [Prime Palindromes](/usaco/pprime)
