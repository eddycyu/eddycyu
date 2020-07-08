---
layout: default
title: "Subset Sums"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, dynamic programming]
order: 2.2.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
For many sets of consecutive integers from 1 through N (1 <= N <= 39), one can 
partition the set into two sets whose sums are identical.

For example, if N=3, one can partition the set {1, 2, 3} in one way so that the
sums of both subsets are identical:

* {3} and {1,2}

This counts as a single partitioning (i.e., reversing the order counts as the 
same partitioning and thus does not increase the count of partitions).

If N=7, there are four ways to partition the set {1, 2, 3, ... 7} so that each 
partition has the same sum:

* {1,6,7} and {2,3,4,5}
* {2,5,7} and {1,3,4,6}
* {3,4,7} and {1,2,5,6}
* {1,2,4,7} and {3,5,6}

Given N, your program should print the number of ways a set containing the 
integers from 1 through N can be partitioned into two sets whose sums are 
identical. Print 0 if there are no such ways.

Your program must calculate the answer, not look it up from a table.

##### PROGRAM NAME: subset

##### INPUT FORMAT
The input file contains a single line with a single integer representing N, 
as above.

##### SAMPLE INPUT (file subset.in)
```
7
```

##### OUTPUT FORMAT
The output file contains a single line with a single integer that tells how 
many same-sum partitions can be made from the set {1, 2, ..., N}. The output 
file should contain 0 if there are no ways to make a same-sum partition.

##### SAMPLE OUTPUT (file subset.out)
```
4
```

### Analysis:
For a set of N numbers (from 1 to N), the sum of the set is N*(N+1)/2 
[(triangular number)](https://en.wikipedia.org/wiki/Triangular_number){:target="_blank" rel="noopener"}. 
Therefore, partitioning the set into two subsets whose sums are identical would 
produce subsets with individual sums of N*(N+1)/4.

The problem can we rephrased from:

Given N, your program should print the number of ways a set containing the 
integers from 1 through N can be partitioned into two sets whose sums are 
identical.

to:

Given N, calculate the total number a partition must sum to {n*(n+1)/4}, 
and find the number of ways to form that sum by adding 1, 2, 3, ... N.

Look at the following example:
```
   SUM:| 0  1  2  3  4  5  6  7  8  9 10 ...
    ------------------------------------
    0  | 1  0  0  0  0  0  0  0  0  0  0 ...       
    1  | 1  1  0  0  0  0  0  0  0  0  0 ...
 N: 2  | 1  1  1  1  0  0  0  0  0  0  0 ...
    3  | 1  1  1  2  1  1  1  0  0  0  0 ...
    4  | 1  1  1  2  2  2  2  2  1  1  1 ...
```
The vertical column (N) is the size of the set, and the horizontal row (SUM) is 
the sum of a subset. The values are the number of ways that a set of size N can
be partitioned into a subset with SUM.
```
For N = 4,
 * there is 1 way to create a subset with a sum of 0: {}
 * there is 1 way to create a subset with a sum of 1: {1}
 * there is 1 way to create a subset with a sum of 2: {2}
 * there are 2 ways to create a subset with a sum of 3: {1, 2} and {3}
 * there are 2 ways to create a subset with a sum of 4: {1, 3} and {4}
 * there are 2 ways to create a subset with a sum of 5: {1, 4} and {2, 3}
 * there are 2 ways to create a subset with a sum of 6: {1, 2, 3} and {2, 4}
 * there are 2 ways to create a subset with a sum of 7: {1, 2, 4} and {3, 4}
 * there is 1 way to create a subset with a sum of 8: {1, 3, 4}
 * there is 1 way to create a subset with a sum of 9: {2, 3, 4}
 * there is 1 way to create a subset with a sum of 10: {1, 2, 3, 4}
```
From the example, we see that we can solve row N+1 from row N, which is a 
classic [(dynamic programming)](https://en.wikipedia.org/wiki/Dynamic_programming){:target="_blank" rel="noopener"}
problem.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class subset {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("subset.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("subset.out")))) {

            final int N = Integer.parseInt(f.readLine());

            // check if any solution exists (even sum only)
            final int totalSum = N * (N + 1) / 2;
            if (totalSum % 2 != 0) {
                out.println(0); // no solution is possible
                return;
            }

            // populate seed data; when N = 0, there is 1 way to create a
            // subset with a sum of 0
            long[] previousN = new long[totalSum + 1];
            previousN[0] = 1;

            // copy value from N-1 to N
            long[] currentN = new long[totalSum + 1];
            System.arraycopy(previousN, 0, currentN, 0, previousN.length - 1);

            // find solution
            for (int n = 1; n <= N; n++) {
                for (int i = 0; i <= (n * (n + 1) / 2); i++) {
                    if (n + i <= totalSum) {
                        // add value from N-1 to N
                        currentN[n + i] += previousN[i];
                    }
                }
                // update from N-1 values with values from N
                System.arraycopy(currentN, 0, previousN, 0, currentN.length - 1);
            }

            // output result (at subset sum, e.g. totalSum/2)
            out.println(currentN[totalSum / 2] / 2); // divide by 2 for double count
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/subset.java){:target="_blank" rel="noopener"}

### Links:
* [Broken Necklace](/usaco/beads)
* [Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming](/blog/find-nth-fibonacci-number-dynamic-programming.html)
