---
layout: default
title: "Dual Palindromes"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.3.5
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
A number that reads the same from right to left as when read from left to right 
is called a [palindrome](https://en.wikipedia.org/wiki/Palindrome){:target="_blank" rel="noopener"}.
The number 12321 is a palindrome; the number 77778 is not. Of course, 
palindromes have neither leading nor trailing zeroes, so 0220 is not a 
palindrome.

The number 21 (base 10) is not palindrome in base 10, but the number 21 
(base 10) is, in fact, a palindrome in base 2 (10101).

Write a program that reads two numbers (expressed in base 10):

* N (1 <= N <= 15)
* S (0 < S < 10000)

and then finds and prints (in base 10) the first N numbers strictly greater 
than S that are palindromic when written in two or more number bases 
(2 <= base <= 10).

Solutions to this problem do not require manipulating integers larger than 
the standard 32 bits.

##### PROGRAM NAME: dualpal

##### INPUT FORMAT
A single line with space separated integers N and S.

##### SAMPLE INPUT (file dualpal.in)
```
3 25
```

##### OUTPUT FORMAT
N lines, each with a base 10 number that is palindromic when expressed in at 
least two of the bases 2..10. The numbers should be listed in order from 
smallest to largest.

##### SAMPLE OUTPUT (file dualpal.out)
```
26
27
28
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

class dualpal {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("dualpal.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("dualpal.out")))) {

            final String line = f.readLine();
            final String[] tokens = line.split(" ");
            final int N = Integer.parseInt(tokens[0]);
            final int S = Integer.parseInt(tokens[1]);

            // iterate through numbers larger than S
            final List<Integer> results = new ArrayList<>();
            for (int number = S + 1; number < Integer.MAX_VALUE; number++) {
                // check if we have already found N numbers strictly greater than S that are palindromic
                if (results.size() == N) {
                    break;
                }
                int countFoundForNumber = 0;
                // iterate through bases to find palindromes
                for (int base = 2; base <= 10; base++) {
                    final String valueBaseN = Integer.toString(number, base);
                    if (isPalindrome(valueBaseN)) {
                        countFoundForNumber++;
                        // check if we have already found palindromes in 2 different bases
                        if (countFoundForNumber == 2) {
                            results.add(number);
                            break;
                        }
                    }
                }
            }

            // output results
            for (Integer result : results) {
                out.println(result);
            }
        }
    }

    private static boolean isPalindrome(String input) {
        final StringBuilder buffer = new StringBuilder(input);
        return (buffer.reverse().toString().equals(input));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/dualpal.java){:target="_blank" rel="noopener"}

### Links:
* [Palindromic Squares](/usaco/palsquare)
* [Check If A String Is A Palindrome](/blog/check-if-a-string-is-a-palindrome)
