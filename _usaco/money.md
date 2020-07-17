---
layout: default
title: "Money Systems"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.3.4
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
The cows have not only created their own government but they have chosen to 
create their own money system. In their own rebellious way, they are curious 
about values of coinage. Traditionally, coins come in values like 1, 5, 10, 20 
or 25, 50, and 100 units, sometimes with a 2 unit coin thrown in for good 
measure.

The cows want to know how many different ways it is possible to dispense a 
certain amount of money using various coin systems. For instance, using a 
system of {1, 2, 5, 10, ...} it is possible to create 18 units several 
different ways, including: 18x1, 9x2, 8x2+2x1, 3x5+2+1, and many others.

Write a program to compute how many ways to construct a given amount of 
money using supplied coinage. It is guaranteed that the total will fit into 
both a signed long (C/C++) and Int64 (Free Pascal).

##### PROGRAM NAME: money

##### INPUT FORMAT
The number of coins in the system is V (1 <= V <= 25).

The amount of money to construct is N (1 <= N <= 10,000).

Line 1:	   |Two integers, V and N
-----------|---------------------
Lines 2..: | V integers that represent the available coins (no particular number of integers per line)

##### SAMPLE INPUT (file money.in)
```
3 10
1 2 5
```

##### OUTPUT FORMAT
A single line containing the total number of ways to construct N money units using V coins.

##### SAMPLE OUTPUT (file money.out)
```
10
```

### Analysis

If you implement this with recursion, you will end up with a solution
that performs too slowly to pass the tests. However, if we example
the results from the sample input, a recurrence can be identified.

Given:
* coins: 1, 2, 5 cents
* amount to construct: 10 cents

We have the following results:

_k_ / _n_ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10
----------|---|---|---|---|---|---|---|---|---|---|----
1         | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1
2         | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5 | 6
3         | 1 | 1 | 2 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 10

where:
* row = first _kth_ coins
* column = sum (_n_)
* value is the number of ways to make _n_ cents with the first _k_ <sup>th</sup> coins
       
For example, table\[9]\[2] = 5 can be read as: 
```
the number of ways to make 9 cents with the first 2 coins is 5
```

If we examine the results in the table, we see the following recurrence:

&nbsp;&nbsp;&nbsp;&nbsp;nways(_n_, _k_) = nways(_n_, _k_ - 1) + nways(_n_ - _kv_, _k_)

&nbsp;&nbsp;&nbsp;&nbsp;where _kv_ is the value of the _k_th coin

In other words, the number of ways to make _n_ cents with _k_ coins is equal to 
the number of ways to make _n_ cents with the first _k_ - 1 coins plus the 
number of ways to make (_n_ - _kv_) cents with the first _k_ coins.

So in our example:
```
nways(10, 3) = nways(10, 2) + nways(7, 3)
             = 6 + 4
             = 10 
```

With this information, we can implement a solution using dynamic programming.

### Solution:
```java
public class money {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("money.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("money.out")))) {
            String[] tokens = f.readLine().split(" ");
            final int V = Integer.parseInt(tokens[0]);  // number of coins in the system
            final int N = Integer.parseInt(tokens[1]);  // amount of money to construct

            // parse coin values
            final int[] coins = new int[V];
            int v = 0;
            String line;
            while ((v < V) && (line = f.readLine()) != null) {
                tokens = line.split(" ");
                for (String token : tokens) {
                    coins[v++] = Integer.parseInt(token);
                }
            }

            // compute using dynamic programming
            final long count = solve(N, coins);

            // output
            out.println(count);
        }
    }

    private static long solve2(int N, int[] coins) {
        // the number of different ways in which to get a sum of N
        final long[] ways = new long[N + 1];

        // base case: the number of different ways in which to get a 
        // sum of 0 is always 1
        ways[0] = 1;

        // loop over every coin value
        for (int coin : coins) {
            // loop over every sum value
            for (int n = 1; n < ways.length; n++) {
                if (n >= coin) {
                    ways[n] += ways[n - coin];
                }
            }
        }

        return ways[N];
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/money.java){:target="_blank" rel="noopener"}
