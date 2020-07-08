---
layout: default
title: "Arithmetic Progressions"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.5.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
An arithmetic progression is a sequence of the form a, a+b, a+2b, ..., a+nb 
where n=0, 1, 2, 3, ... . For this problem, a is a non-negative integer and b 
is a positive integer.

Write a program that finds all arithmetic progressions of length n in the set S
of bisquares. The set of bisquares is defined as the set of all integers of the 
form p2 + q2 (where p and q are non-negative integers).

##### PROGRAM NAME: ariprog

##### INPUT FORMAT

Line 1: | N (3 <= N <= 25), the length of progressions for which to search
--------|---------------------
Line 2: | M (1 <= M <= 250), an upper bound to limit the search to the bisquares with 0 <= p,q <= M

##### SAMPLE INPUT (file ariprog.in)
```
5
7
```

##### OUTPUT FORMAT
If no sequence is found, a single line reading `NONE'. Otherwise, output one or 
more lines, each with two integers: the first element in a found sequence and 
the difference between consecutive elements in the same sequence. The lines 
should be ordered with smallest-difference sequences first and smallest 
starting number within those sequences first.

There will be no more than 10,000 sequences.

##### SAMPLE OUTPUT (file ariprog.out)
```
1 4
37 4
2 8
29 8
1 12
5 12
13 12
17 12
5 20
2 24
```

### Analysis:
We can use a [brute force](https://en.wikipedia.org/wiki/Brute-force_search){:target="_blank" rel="noopener"} 
approach to solve this problem.
    
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
import java.util.List;

class ariprog {

    public static void main(String[] args) throws IOException {

        try (final BufferedReader f = new BufferedReader(new FileReader("ariprog.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("ariprog.out")))) {

            // length of arithmetic progression to find
            final int N = Integer.parseInt(f.readLine());

            // upper bound to limit the search to the bisquares
            final int M = Integer.parseInt(f.readLine());

            // find all possible bisquares for allowed values in arithmetic progression
            final boolean[] bisquares = getBisquares(M);

            // maximum bisquare value, which will be the upper bound for 'a' and 'b' in the
            // arithmetic progression equation (a, a+b, a+2b, ..., a+nb)
            final int maxBisquare = M * M + M * M;

            // search for valid arithmetic progressions
            final List<Result> results = getResults(N, maxBisquare, bisquares);

            // sort results
            Collections.sort(results);

            // output results
            if (results.size() == 0) {
                out.println("NONE");
            } else {
                for (Result result : results) {
                    out.print(result.first);
                    out.print(" ");
                    out.print(result.diff);
                    out.println();
                }
            }
        }
    }

    /**
     * Find all possible bisquares (p*p + q*q).
     */
    private static boolean[] getBisquares(int M) {
        final boolean[] result = new boolean[M * M + M * M + 1];
        for (int p = 0; p <= M; p++) {
            for (int q = 0; q <= M; q++) {
                result[(p * p) + (q * q)] = true;
            }
        }
        return result;
    }

    /**
     * Get the results for all valid arithmetic progressions of length N whose
     * values are in the set of bisquares.
     */
    private static List<Result> getResults(int N, int maxBisquare, boolean[] bisquares) {
        final List<Result> results = new ArrayList<>();
        for (int a = 0; a <= maxBisquare / 2; a++) {
            for (int b = 1; b <= maxBisquare / 2; b++) {
                // optimization: eliminate loops that would result in
                // invalid arithmetic progression
                int lastValue = (a + ((N - 1) * b));
                if ((lastValue > maxBisquare)) {
                    break;
                } else if (!bisquares[lastValue]) {
                    continue;
                }
                final int[] progression = getProgression(N, a, b, bisquares);
                // a value of zero for the last element in the sequence would
                // indicate that this is not a valid arithmetic progression
                if (progression[N - 1] != 0) {
                    final int first = progression[0];
                    final int diff = progression[1] - progression[0];
                    results.add(new Result(first, diff));
                }
                // else discard the invalid progression
            }
        }
        return results;
    }

    /**
     * Get arithmetic progression (a + nb) where values are in the set of bisquares.
     */
    private static int[] getProgression(int N, int a, int b, boolean[] bisquares) {
        final int[] progression = new int[N];
        for (int n = 0; n < N; n++) {
            int value = a + (n * b);
            if (bisquares[value]) {
                progression[n] = value;
            } else {
                // optimization: terminate loop and return incomplete progression
                break;
            }
        }
        return progression;
    }

    public static class Result implements Comparable<Result> {
        private int first;
        private int diff;

        Result(int first, int diff) {
            super();
            this.first = first;
            this.diff = diff;
        }

        @Override
        public int compareTo(Result other) {
            if (diff < other.diff) {
                return -1;
            } else if (diff > other.diff) {
                return 1;
            } else {
                if (first < other.first) {
                    return -1;
                } else if (first > other.first) {
                    return 1;
                }
                return 0;
            }
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/ariprog.java){:target="_blank" rel="noopener"}

### Links:
* [Palindromic Squares](/usaco/palsquare)
* [Check If A String Is A Palindrome](/blog/check-if-a-string-is-a-palindrome)
