---
layout: default
title: "Palindromic Squares"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, palindrome]
order: 1.3.4
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
[Palindromes](https://en.wikipedia.org/wiki/Palindrome){:target="_blank" rel="noopener"} 
are numbers that read the same forwards as backwards. The number 12321 is a 
typical palindrome.

Given a number base B (2 <= B <= 20 base 10), print all the integers N 
(1 <= N <= 300 base 10) such that the square of N is palindromic when 
expressed in base B; also print the value of that palindromic square. 
Use the letters 'A', 'B', and so on to represent the digits 10, 11, and so on.

Print both the number and its square in base B.

##### PROGRAM NAME: palsquare

##### INPUT FORMAT
A single line with B, the base (specified in base 10).

##### SAMPLE INPUT (file palsquare.in)
```
10
```

##### OUTPUT FORMAT
Lines with two integers represented in base B. The first integer is the number 
whose square is palindromic; the second integer is the square itself. 
NOTE WELL THAT BOTH INTEGERS ARE IN BASE B!

##### SAMPLE OUTPUT (file palsquare.out)
```
1 1
2 4
3 9
11 121
22 484
26 676
101 10201
111 12321
121 14641
202 40804
212 44944
264 69696
```

### Analysis:
Brute force works here.

First we write a function to check if a given input is a palindrome. To check 
if an input is a palindrome, we just need to compare if the input is the same 
as the reverse of the input. 

Then, the process would be to iterate through the numbers from 1 to 300 
(base 10), compute the square of the number, convert the square of the number
to base B, and check if the converted base B number is a palindrome.
    
### Solution:
```java
class palsquare {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("palsquare.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("palsquare.out")))) {

            // base
            final int B = Integer.parseInt(f.readLine());

            // iterate through all numbers from 1 to 300 to find palindrome
            for (int n = 1; n <= 300; n++) {
                final int squaredValue = n * n;
                final String squaredValueBaseN = Integer.toString(squaredValue, B);
                if (isPalindrome(squaredValueBaseN)) {
                    final String indexBaseN = Integer.toString(n, B);
                    out.println(indexBaseN.toUpperCase() + " " + squaredValueBaseN.toUpperCase());
                }
            }
        }
    }

    private static boolean isPalindrome(String input) {
        final StringBuilder buffer = new StringBuilder(input);
        return (buffer.reverse().toString().equals(input));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/palsquare.java){:target="_blank" rel="noopener"}

### Links:
* [Dual Palindromes](/usaco/dualpal)
* [Check If A String Is A Palindrome](/blog/check-if-a-string-is-a-palindrome)
