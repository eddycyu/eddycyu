---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 6: Recursion - Trinomial Coefficients (Dynamic Programming)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.6.2
---

Write a program TrinomialDP.java that takes two integer command-line arguments 
_n_ and _k_ and computes the trinomial coefficient T(_n_, _k_) using dynamic 
programming. To do so, organize your program according to the following public 
API:

```java
public class TrinomialDP {

    // Returns the trinomial coefficient T(n, k).
    public static long trinomial(int n, int k)

    // Takes two integer command-line arguments n and k and prints T(n, k).
    public static void main(String[] args)
}
```

This version should be fast enough to handle larger values of _n_ and _k_.

```
~/Desktop/recursion> java TrinomialDP 3 0
7

~/Desktop/recursion> java TrinomialDP 24 12
287134346

~/Desktop/recursion> java TrinomialDP 30 0
18252025766941

~/Desktop/recursion> java TrinomialDP 40 0
934837217271732457
```
  
##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/recursion/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class TrinomialDP {

    // Returns the trinomial coefficient T(n, k).
    public static long trinomial(int n, int k) {

        if ((k > n) || (k < -n)) {
            return 0;
        }

        final long[][] dp = new long[n + 1][n + 1];
        dp[0][0] = 1; // if n=0 && k=0 then T(n,k)=1
        for (int i = 1; i < n + 1; i++) {
            for (int j = i; j >= 0; j--) {
                long a; // T(n-1,k-1)
                if (j - 1 >= 0) {
                    a = dp[i - 1][j - 1];
                } else {
                    // T(n,k) = T(n,-k)
                    a = dp[i - 1][Math.abs(j - 1)];
                }
                final long b = dp[i - 1][j]; // T(n-1,k)
                final long c;
                if (j < i) {
                    c = dp[i - 1][j + 1]; // T(n-1,k+1)
                } else {
                    c = 0; // if k > n then 0
                }
                dp[i][j] = a + b + c;
            }
        }
        return dp[n][Math.abs(k)];
    }

    // Takes two integer command-line arguments n and k and prints T(n, k).
    public static void main(String[] args) {
        final int n = Integer.parseInt(args[0]);
        final int k = Integer.parseInt(args[1]);
        StdOut.println(trinomial(n, k));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/TrinomialDP.java){:target="_blank" rel="noopener"}
