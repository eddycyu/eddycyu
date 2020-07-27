---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 7: Performance - Ramanujan Numbers"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.7.2
---

When the English mathematician G. H. Hardy came to visit the Indian 
mathematician Srinivasa Ramanujan in the hospital one day, Hardy 
remarked that the number of his taxi was 1729, a rather dull number. 
To which Ramanujan replied, No, Hardy! No, Hardy! It is a very 
interesting number; it is the smallest number expressible as the sum 
of two cubes in two different ways.

An integer n is a [Ramanujan number](https://en.wikipedia.org/wiki/Taxicab_number){:target="_blank" rel="noopener"}
if it can be expressed as the sum of two positive cubes in two different ways. 
That is, there are four distinct positive integers _a_, _b_, _c_, and _d_ 
such that _n_ = _a_ <sup>3</sup> + _b_ <sup>3</sup> = _c_ <sup>3</sup> + _d_ <sup>3</sup>. 
For example 1729 = 13 + 123 = 93 + 103.

Write a program Ramanujan.java that takes a long integer command-line 
argument _n_ and prints _true_ if it is a Ramanujan number, and _false_
otherwise. To do so, organize your program according to the following public 
API:

```java
public class Ramanujan {

    // Is n a Ramanujan number?
    public static boolean isRamanujan(long n)

    // Takes a long integer command-line arguments n and prints true if
    // n is a Ramanujan number, and false otherwise.
    public static void main(String[] args)
}
```

Here are a few sample executions:

```
~/Desktop/performance> java Ramanujan 1729
true

~/Desktop/performance> java Ramanujan 3458
false

~/Desktop/performance> java Ramanujan 4104
true

~/Desktop/performance> java Ramanujan 216125
true

~/Desktop/performance> java Ramanujan 9223278330318728221
true
```

Your program should take time proportional to n<sup>1/3</sup> in the worst 
case. It should be fast enough to process any 64-bit long integer in a 
fraction of a second.

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/performance/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class Ramanujan {

    // Is n a Ramanujan number?
    public static boolean isRamanujan(long n) {
        long firstA = 0;
        long firstB = 0;
        int count = 0;

        // if minA = 1, then maxA cannot be larger than cbrt(n)
        final long minA = 1;
        final long maxA = (long) Math.cbrt(n);
        for (long a = minA; a < maxA; a++) {
            final long curA = a * a * a;
            // if curA, then the only possible value for b is cbrt(n - curA)
            final long b = (long) Math.cbrt(n - curA);
            final long curB = b * b * b;
            final long ab = curA + curB;
            if (ab == n) {
                if (count == 0) {
                    count++;
                    firstA = a;
                    firstB = b;
                } else if ((firstA != b) && (firstB != a)) { // make sure not reversed
                    count++;
                }
                if (count > 1) {
                    return true; // found two positive cubes in two different ways
                }
            }
        }
        return false;
    }

    // Takes a long integer command-line arguments n and prints true if
    // n is a Ramanujan number, and false otherwise.
    public static void main(String[] args) {
        final long n = Long.parseLong(args[0]);
        StdOut.print(isRamanujan(n));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/Ramanujan.java){:target="_blank" rel="noopener"}
