---
layout: default
title: "Prime Palindromes"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, palindrome, prime]
order: 1.6.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
The number 151 is a [prime palindrome](https://en.wikipedia.org/wiki/Palindromic_prime){:target="_blank" rel="noopener"}
because it is both a prime number and a palindrome (it is the same number when 
read forward as backward). Write a program that finds all prime palindromes in 
the range of two supplied numbers a and b (5 <= a < b <= 100,000,000); both a 
and b are considered to be within the range.

##### PROGRAM NAME: pprime

##### INPUT FORMAT
Line 1:	Two integers, a and b

##### SAMPLE INPUT (file pprime.in)
```
5 500
```

##### OUTPUT FORMAT
The list of palindromic primes in numerical order, one per line.

##### SAMPLE OUTPUT (file pprime.out)
```
5
7
11
101
131
151
181
191
313
353
373
383
```

##### HINT 1
Generate the palindromes and see if they are prime.

##### HINT 2
Generate palindromes by combining digits properly. You might need more than one 
of the loops like below.
```
/* generate five digit palindrome: */
for (d1 = 1; d1 <= 9; d1+=2) {	/* only odd; evens aren't so prime */
    for (d2 = 0; d2 <= 9; d2++) {
        for (d3 = 0; d3 <= 9; d3++) {
	    palindrome = 10000*d1 + 1000*d2 +100*d3 + 10*d2 + d1;
	    ... deal with palindrome ...
        }
    }
}
```

### Analysis:
Brute force works here.

First we write a function to check if a given input is a palindrome. To check 
if an input is a palindrome, we just need to compare if the input is the same 
as the reverse of the input. 

Then, the process would be to iterate through the numbers (base 10) that are 
greater than S, iterate through the bases from 2 to 10, convert the number to
the base and check if it is a palindrome. Continue until you find the first N
numbers that are palindromic when written in two or more bases.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

public class pprime {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("pprime.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("pprime.out")))) {

            final String line = f.readLine();
            final String[] tokens = line.split(" ");
            final int a = Integer.parseInt(tokens[0]);
            final int b = Integer.parseInt(tokens[1]);

            // determine the lowest and highest number of digits for palindromes
            int lowestDigits = getNumDigits(a);
            int highestDigits = getNumDigits(b);

            // find and output prime palindromes within the range [a,b]
            for (int i = lowestDigits; i <= highestDigits; i++) {
                List<Integer> palindromes = getPrimePalindromes(i, a, b);
                for (Integer palindrome : palindromes) {
                    out.println(palindrome);
                }
            }
        }
    }

    private static int getNumDigits(int number) {
        // 'a' and 'b' from 5 to 100,000,000...
        // so valid prime palindromes have 1 to 8 digits...
        if ((number >= 1) && (number < 10)) {
            return 1;
        } else if ((number > 10) && (number <= 100)) {
            return 2;
        } else if ((number > 100) && (number <= 1000)) {
            return 3;
        } else if ((number > 1000) && (number <= 10000)) {
            return 4;
        } else if ((number > 10000) && (number <= 100000)) {
            return 5;
        } else if ((number > 100000) && (number <= 1000000)) {
            return 6;
        } else if ((number > 1000000) && (number <= 10000000)) {
            return 7;
        } else if ((number > 10000000) && (number <= 100000000)) {
            return 8;
        }
        return 0;
    }

    // this needs to be rewritten!
    private static List<Integer> getPrimePalindromes(int digits, int lower, int upper) {
        final List<Integer> palindromes = new ArrayList<>();
        switch (digits) {
            case 1: {
                // generate one digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    if ((d1 >= lower) && (d1 <= upper)) {
                        if (isPrime(d1)) {
                            palindromes.add(d1);
                        }
                    }
                }
                break;
            }
            case 2: {
                // generate two digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    int palindrome = 10 * d1 + d1;
                    if ((palindrome >= lower) && (palindrome <= upper)) {
                        if (isPrime(palindrome)) {
                            palindromes.add(palindrome);
                        }
                    }
                }
                break;
            }
            case 3: {
                // generate three digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        int palindrome = 100 * d1 + 10 * d2 + d1;
                        if ((palindrome >= lower) && (palindrome <= upper)) {
                            if (isPrime(palindrome)) {
                                palindromes.add(palindrome);
                            }
                        }
                    }
                }
                break;
            }
            case 4: {
                // generate four digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        int palindrome = 1000 * d1 + 100 * d2 + 10 * d2 + d1;
                        if ((palindrome >= lower) && (palindrome <= upper)) {
                            if (isPrime(palindrome)) {
                                palindromes.add(palindrome);
                            }
                        }
                    }
                }
                break;
            }
            case 5: {
                // generate five digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        for (int d3 = 0; d3 <= 9; d3++) {
                            int palindrome = 10000 * d1 + 1000 * d2 + 100 * d3 + 10 * d2 + d1;
                            if ((palindrome >= lower) && (palindrome <= upper)) {
                                if (isPrime(palindrome)) {
                                    palindromes.add(palindrome);
                                }
                            }
                        }
                    }
                }
                break;
            }
            case 6: {
                // generate six digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        for (int d3 = 0; d3 <= 9; d3++) {
                            int palindrome = 100000 * d1 + 10000 * d2 + 1000
                                    * d3 + 100 * d3 + 10 * d2 + d1;
                            if ((palindrome >= lower) && (palindrome <= upper)) {
                                if (isPrime(palindrome)) {
                                    palindromes.add(palindrome);
                                }
                            }
                        }
                    }
                }
                break;
            }
            case 7: {
                // generate seven digit palindrome:
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        for (int d3 = 0; d3 <= 9; d3++) {
                            for (int d4 = 0; d4 <= 9; d4++) {
                                int palindrome = 1000000 * d1 + 100000 * d2 + 10000
                                        * d3 + 1000 * d4 + 100 * d3 + 10 * d2 + d1;
                                if ((palindrome >= lower) && (palindrome <= upper)) {
                                    if (isPrime(palindrome)) {
                                        palindromes.add(palindrome);
                                    }
                                }
                            }
                        }
                    }
                }
                break;
            }
            case 8: {
                // generate eight digit palindrome
                for (int d1 = 1; d1 <= 9; d1 += 2) { // only odd; evens aren't so prime
                    for (int d2 = 0; d2 <= 9; d2++) {
                        for (int d3 = 0; d3 <= 9; d3++) {
                            for (int d4 = 0; d4 <= 9; d4++) {
                                int palindrome = 10000000 * d1 + 1000000 * d2 + 100000
                                        * d3 + 10000 * d4 + 1000 * d4 + 100 * d3 + 10
                                        * d2 + d1;
                                if ((palindrome >= lower) && (palindrome <= upper)) {
                                    if (isPrime(palindrome)) {
                                        palindromes.add(palindrome);
                                    }
                                }
                            }
                        }
                    }
                }
                break;
            }
        }
        return palindromes;
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
* [Palindromic Squares](/usaco/palsquare)
* [Check If A String Is A Palindrome](/blog/check-if-a-string-is-a-palindrome)
* [Check If A Number Is Prime](/blog/check-if-a-number-is-prime)