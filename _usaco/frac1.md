---
layout: default
title: "Ordered Fractions"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.1.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Consider the set of all reduced fractions between 0 and 1 inclusive with 
denominators less than or equal to N.

Here is the set when N = 5:
```
0/1 1/5 1/4 1/3 2/5 1/2 3/5 2/3 3/4 4/5 1/1
```
Write a program that, given an integer N between 1 and 160 inclusive, prints 
the fractions in order of increasing magnitude.

##### PROGRAM NAME: frac1

##### INPUT FORMAT
One line with a single integer N.

##### SAMPLE INPUT (file frac1.in)
```
5
```

##### OUTPUT FORMAT
One fraction per line, sorted in order of magnitude.

##### SAMPLE OUTPUT (file frac1.out)
```
0/1
1/5
1/4
1/3
2/5
1/2
3/5
2/3
3/4
4/5
1/1
```
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class frac1 {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("frac1.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("frac1.out")))) {

            final int N = Integer.parseInt(f.readLine());

            // generate the fractions
            final Set<Fraction> fractions = new HashSet<>();
            for (int denominator = 1; denominator <= N; denominator++) {
                for (int numerator = 0; numerator <= denominator; numerator++) {
                    fractions.add(new Fraction(numerator, denominator));
                }
            }

            // sort the results
            final List<Fraction> sortedFractions = new ArrayList<>(fractions);
            Collections.sort(sortedFractions);

            // output results
            for (Fraction fraction : sortedFractions) {
                out.println(fraction.numerator + "/" + fraction.denominator);
            }
        }
    }

    static class Fraction implements Comparable<Fraction> {
        int numerator;
        int denominator;
        int value;

        private Fraction() {
            super();
        }

        Fraction(int numerator, int denominator) {
            this();
            this.numerator = numerator;
            this.denominator = denominator;

            // since numbers are from 1 to 160, multiply by 100000 to avoid decimals
            // with enough precision
            this.value = (int) (100000 * ((float) numerator / (float) denominator));
        }

        @Override
        public boolean equals(Object obj) {
            if (obj == null) {
                return false;
            }

            if (!Fraction.class.isAssignableFrom(obj.getClass())) {
                return false;
            }

            final Fraction other = (Fraction) obj;
            return this.value == other.value;
        }

        @Override
        public int hashCode() {
            int hash = 3;
            hash = 53 * hash + this.value;
            return hash;
        }

        @Override
        public int compareTo(Fraction other) {
            if (this.value < other.value) {
                return -1;
            } else if (this.value > other.value) {
                return 1;
            }
            return 0;
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/frac1.java){:target="_blank" rel="noopener"}
