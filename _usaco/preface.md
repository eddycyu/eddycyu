---
layout: default
title: "Preface Numbering"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.2.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
A certain book's prefaces are numbered in upper case Roman numerals. 
Traditional Roman numeral values use a single letter to represent a 
certain subset of decimal numbers. Here is the standard set:
```
I   1     L   50    M  1000
V   5     C  100
X  10     D  500
```
As many as three of the same marks that represent 10<sup>n</sup> may be placed 
consecutively to form other numbers:

* III is 3
* CCC is 300

Any mark that has the value 5x10<sup>n</sup> can not be used consecutively.

Generally (with the exception of the next rule), marks are connected together 
and written in descending order to form even more numbers:

* CCLXVIII = 100+100+50+10+5+1+1+1 = 268

Sometimes, a mark that represents 10<sup>n</sup> is placed before a mark of one of the 
two next higher values (I before V or X; X before L or C; etc.). In this case, 
the value of the smaller mark is SUBTRACTED from the mark it precedes:

* IV = 4
* IX = 9
* XL = 40
 
This compound mark forms a unit and may not be combined to make another 
compound mark (e.g., IXL is wrong for 39; XXXIX is correct).

Compound marks like XD, IC, and XM are not legal, since the smaller mark is too 
much smaller than the larger one. For XD (wrong for 490), one would use CDXC; 
for IC (wrong for 99), one would use XCIX; for XM (wrong for 990), one would use 
CMXC. 90 is expressed XC and not LXL, since L followed by X connotes that 
successive marks are X or smaller (probably, anyway).

Given N (1 <= N < 3,500), the number of pages in the preface of a book, 
calculate and print the number of I's, V's, etc. (in order from lowest to 
highest) required to typeset all the page numbers (in Roman numerals) from 1 
through N. Do not print letters that do not appear in the page numbers 
specified.

If N = 5, then the page numbers are: I, II, III, IV, V. The total number of 
I's is 7 and the total number of V's is 2.

##### PROGRAM NAME: preface

##### INPUT FORMAT
A single line containing the integer N.

##### SAMPLE INPUT (file preface.in)
```
5
```

##### OUTPUT FORMAT
The output lines specify, in ascending order of Roman numeral letters, the 
letter, a single space, and the number of times that letter appears on 
preface page numbers. Stop printing letter totals after printing the highest 
value letter used to form preface numbers in the specified set.

##### SAMPLE OUTPUT (file preface.out)
```
I 7
V 2
```
    
### Solution:
```java
public class preface {

    final static private int I = 1;
    final static private int V = 5;
    final static private int X = 10;
    final static private int L = 50;
    final static private int C = 100;
    final static private int D = 500;
    final static private int M = 1000;

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("preface.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("preface.out")))) {

            // number of pages in the preface of a book
            final int N = Integer.parseInt(f.readLine());

            // 0 = I, 1 = V, 2 = X, 3 = L, 4 = C, 5 = D, 6 = M
            int[] counts = new int[7];

            // count number of roman numeral letters
            for (int page = 1; page <= N; page++) {
                countLetters(page, counts);
            }

            // output
            if (counts[0] > 0) {
                out.println("I " + counts[0]);
            }
            if (counts[1] > 0) {
                out.println("V " + counts[1]);
            }
            if (counts[2] > 0) {
                out.println("X " + counts[2]);
            }
            if (counts[3] > 0) {
                out.println("L " + counts[3]);
            }
            if (counts[4] > 0) {
                out.println("C " + counts[4]);
            }
            if (counts[5] > 0) {
                out.println("D " + counts[5]);
            }
            if (counts[6] > 0) {
                out.println("M " + counts[6]);
            }
        }
    }

    private static void countLetters(int number, int[] counts) {
        while (number > 0) {
            if (number >= M) {
                number -= M;
                counts[6]++; // M
            } else if (number >= D) {
                if (number >= 900) {
                    number -= 900;
                    counts[4]++; // C
                    counts[6]++; // M
                } else {
                    number -= D;
                    counts[5]++; // D
                }
            } else if (number >= C) {
                if (number >= 400) {
                    number -= 400;
                    counts[4]++; // C
                    counts[5]++; // D
                } else {
                    number -= C;
                    counts[4]++; // C
                }
            } else if (number >= L) {
                if (number >= 90) {
                    number -= 90;
                    counts[2]++; // X
                    counts[4]++; // C
                } else {
                    number -= L;
                    counts[3]++; // L
                }
            } else if (number >= X) {
                if ((number >= 40)) {
                    number -= 40;
                    counts[2]++; // X
                    counts[3]++; // L
                } else {
                    number -= X;
                    counts[2]++; // X
                }
            } else if (number >= V) {
                if (number >= 9) {
                    number -= 9;
                    counts[0]++; // I
                    counts[2]++; // X
                } else {
                    number -= V;
                    counts[1]++; // V
                }
            } else {
                if (number >= 4) {
                    number -= 4;
                    counts[0]++; // I
                    counts[1]++; // V
                } else {
                    number -= I;
                    counts[0]++; // I
                }
            }
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/preface.java){:target="_blank" rel="noopener"}
