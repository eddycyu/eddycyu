---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 2: Conditionals And Loops - Generalized Harmonic Numbers"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.2.1
---

Write a program GeneralizedHarmonic.java that takes two integer command-line 
arguments _n_ and _r_ and uses a for loop to compute the _n_<sup>th</sup> 
_generalized harmonic number_ of order _r_, which is defined by the following 
formula:

&nbsp;&nbsp;&nbsp;&nbsp;H(n, r) = 1 / 1<sup>r</sup> + 1 / 2<sup>r</sup> + ⋯ + 1 / n<sup>r</sup>.

For example, H(3, 2) = 1 / 1<sup>2</sup> + 1 / 2<sup>2</sup> + 1 / 3<sup>2</sup> = 4936 ≈ 1.361111.

```
~/Desktop/loops> java GeneralizedHarmonic 1 1
1.0

~/Desktop/loops> java GeneralizedHarmonic 2 1
1.5

~/Desktop/loops> java GeneralizedHarmonic 3 1
1.8333333333333333

~/Desktop/loops> java GeneralizedHarmonic 1 2
1.0

~/Desktop/loops> java GeneralizedHarmonic 2 2
1.25

~/Desktop/loops> java GeneralizedHarmonic 3 2
1.3611111111111112
```

_Note:_ you may assume that _n_ is a positive integer.

The generalized harmonic numbers are closely related to the Riemann zeta 
function, which plays a central role in number theory.

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/loops/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class GeneralizedHarmonic {

    public static void main(String[] args) {
        final int n = Integer.parseInt(args[0]);
        final int r = Integer.parseInt(args[1]);

        double sum = 0.0;
        for (int i = 1; i <= n; i++) {
            sum = sum + (1 / Math.pow(i, r));
        }
        System.out.println(sum);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/GeneralizedHarmonic.java){:target="_blank" rel="noopener"}
