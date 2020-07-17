---
layout: default
title: "Hamming Codes"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.1.5
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Given N, B, and D: Find a set of N codewords (1 <= N <= 64), each of length B 
bits (1 <= B <= 8), such that each of the codewords is at least Hamming 
distance of D (1 <= D <= 7) away from each of the other codewords.

The Hamming distance between a pair of codewords is the number of binary bits 
that differ in their binary notation. Consider the two codewords 0x554 and 
0x234 and their differences (0x554 means the hexadecimal number with hex digits 
5, 5, and 4)(a hex digit requires four bits):
```
           0x554 = 0101 0101 0100
           0x234 = 0010 0011 0100
Bit differences:   -XXX -XX- ----
```
Since five bits were different, the Hamming distance is 5.

##### PROGRAM NAME: hamming

##### INPUT FORMAT
N, B, D on a single line

##### SAMPLE INPUT (file hamming.in)
```
16 7 3
```

##### OUTPUT FORMAT
N codewords, sorted, in decimal, ten per line. In the case of multiple 
solutions, your program should output the solution which, if interpreted 
as a base 2^B integer, would have the least value.

##### SAMPLE OUTPUT (file hamming.out)
```
0 7 25 30 42 45 51 52 75 76
82 85 97 102 120 127
```
    
### Solution:
```java
public class hamming {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("hamming.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("hamming.out")))) {

            // read input
            final String[] tokens = f.readLine().split(" ");
            final int N = Integer.parseInt(tokens[0]);
            final int B = Integer.parseInt(tokens[1]);
            final int D = Integer.parseInt(tokens[2]);
            final int maxCodewordValue = (int) Math.pow(2, B) - 1;

            // find solution
            final int[] codewords = new int[maxCodewordValue + 1];
            for (int i = 0; i < codewords.length; i++) {
                codewords[i] = i;
            }
            for (int i = 0; i < codewords.length; i++) {
                final List<Integer> result = new ArrayList<>();
                result.add(codewords[i]);
                findSolution(codewords[i], Arrays.copyOfRange(codewords, i + 1, codewords.length), N, B, D, result);
                // we're guaranteed to find a solution; print the first one we find
                if (result.size() >= N) {
                    for (int j = 0; j < result.size(); j++) {
                        out.print(result.get(j));
                        if (((j + 1) % 10 == 0) || (j == result.size() - 1)) {
                            out.println();
                        } else if (j < result.size() - 1) {
                            out.print(" ");
                        }
                    }
                    break;
                }
            }
        }
    }

    private static void findSolution(int baseCodeword, int[] codewords,
                                     int maxCodewords, int maxBits, int hammingDistance,
                                     List<Integer> result) {
        final int[] subset = new int[codewords.length];
        Arrays.fill(subset, -1);

        // filter out the codewords (into subset) that are hamming distance
        // from baseCodeword
        int index = 0;
        for (int codeword : codewords) {
            if (isHamming(baseCodeword, codeword, maxBits, hammingDistance)) {
                subset[index++] = codeword;
            }
        }

        // if subset is not empty, then add the first value from subset into result
        if (subset[0] != -1) {
            result.add(subset[0]);
            // optimization: don't need to find more than required solution size
            if (result.size() == maxCodewords) {
                return;
            }
        }

        // continue recursion
        if (subset[1] != -1) {
            findSolution(subset[0], Arrays.copyOfRange(subset, 1, index),
                    maxCodewords, maxBits, hammingDistance, result);
        }
    }

    /**
     * Check if codewordA is hamming distance from codewordB.
     */
    private static boolean isHamming(int codewordA, int codewordB, int maxBits, int hammingDistance) {
        int count = 0;
        for (int i = 0; i < maxBits; i++) {
            final int bit = 1 << i;
            final boolean a = (codewordA & bit) == bit;
            final boolean b = (codewordB & bit) == bit;
            if ((a && !b) || (!a && b)) {
                if (++count == hammingDistance) {
                    return true;
                }
            }
        }
        return false;
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/hamming.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion)
