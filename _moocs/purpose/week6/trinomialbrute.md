---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 6: Recursion - Trinomial Coefficients (Brute Force)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.6.1
---

Write a program TrinomialBrute.java that takes two integer command-line 
arguments _n_ and _k_ and computes the corresponding trinomial 
coefficient. The trinomial coefficient T(_n_, _k_) is the coefficient of 
_x<sup>n+k</sup>_ in the expansion of (1 + _x_ + _x<sup>2</sup>_)<sup>_n_</sup>. 
For example,

&nbsp;&nbsp;&nbsp;&nbsp;(1 + _x_ + _x<sup>2</sup>_)<sup>3</sup> = 1 + 3_x_ + 6_x<sup>2</sup>_ + 7_x<sup>3</sup>_ + 6_x<sup>4</sup>_ + 3_x<sup>5</sup>_ + _x<sup>6</sup>_

Thus, T(3, 3) = 1, T(3, 2) = 3, T(3, 1) = 6, and T(3, 0) = 7.

Implement a recursive function _trinomial()_ to compute T(_n_, _k_) by using 
the following recurrence relation:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if _n_ = 0 and _k_ = 0<br/>
&nbsp;&nbsp;&nbsp;&nbsp;T(_n_, _k_) = 0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if _k_ < -_n_ or _k_ > _n_<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;T(_n_-1, _k_-1) + T(_n_-1, _k_) + T(_n_-1, _k_+1)&nbsp;&nbsp;&nbsp;otherwise

To do so, organize your program according to the following public API:

```java
public class TrinomialBrute {

    // Returns the trinomial coefficient T(n, k).
    public static long trinomial(int n, int k)

    // Takes two integer command-line arguments n and k and prints T(n, k).
    public static void main(String[] args)
}
```

As you will see, this approach is hopeless slow for moderately large values of
_n_ and _k_.

```
~/Desktop/recursion> java TrinomialBrute 3 3
1

~/Desktop/recursion> java TrinomialBrute 3 2
3

~/Desktop/recursion> java TrinomialBrute 3 1
6

~/Desktop/recursion> java TrinomialBrute 3 0
7

~/Desktop/recursion> java TrinomialBrute 24 12
287134346

~/Desktop/recursion> java TrinomialBrute 30 0
[takes too long]
```

Note: you may assume that _n_ is a non-negative integer.

Trinomial coefficients arise in combinatorics. For example, T(_n_, _k_) is the 
number of permutations of _n_ symbols, each of which is −1, 0, or +1, which 
sum to exactly _k_ and T(_n_, _k−n_) is the number of different ways of 
randomly drawing _k_ cards from two identical decks of _n_ playing cards.
  
##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/recursion/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class TrinomialBrute {

    // Returns the trinomial coefficient T(n, k).
    public static long trinomial(int n, int k) {
        if ((n == 0) && (k == 0)) {
            return 1;
        } else if ((k < -n) || (k > n)) {
            return 0;
        }
        return trinomial(n - 1, k - 1) + trinomial(n - 1, k) + trinomial(n - 1, k + 1);
    }

    // Takes two integer command-line arguments n and k and prints T(n, k).
    public static void main(String[] args) {
        final int n = Integer.parseInt(args[0]);
        final int k = Integer.parseInt(args[1]);
        StdOut.println(trinomial(n, k));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/TrinomialBrute.java){:target="_blank" rel="noopener"}
