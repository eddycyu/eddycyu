---
layout: default
title: "Broken Necklace"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, dynamic programming]
order: 1.2.4
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
You have a necklace of N red, white, or blue beads (3<=N<=350) some of which 
are red, others blue, and others white, arranged at random. Here are two 
examples for n=29:

                1 2                               1 2
            r b b r                           b r r b
          r         b                       b         b
         r           r                     b           r
        r             r                   w             r
       b               r                 w               w
      b                 b               r                 r
      b                 b               b                 b
      b                 b               r                 b
       r               r                 b               r
        b             r                   r             r
         b           r                     r           r
           r       r                         r       b
             r b r                             r r w
            Figure A                         Figure B
                        r red bead
                        b blue bead
                        w white bead

The beads considered first and second in the text that follows have been marked
in the picture.

The configuration in Figure A may be represented as a string of b's and r's, 
where b represents a blue bead and r represents a red one, as follows: 
brbrrrbbbrrrrrbrrbbrbbbbrrrrb .

Suppose you are to break the necklace at some point, lay it out straight, and 
then collect beads of the same color from one end until you reach a bead of a 
different color, and do the same for the other end (which might not be of the 
same color as the beads collected before this).

Determine the point where the necklace should be broken so that the most 
number of beads can be collected.

Example:

For example, for the necklace in Figure A, 8 beads can be collected, with the 
breaking point either between bead 9 and bead 10 or else between bead 24 and 
bead 25.

In some necklaces, white beads had been included as shown in Figure B above. 
**When collecting beads, a white bead that is encountered may be treated as 
either red or blue and then painted with the desired color.** The string that 
represents this configuration can include any of the three symbols r, b and w.

Write a program to determine the largest number of beads that can be collected 
from a supplied necklace.

##### PROGRAM NAME: beads

##### INPUT FORMAT
Line 1:	N, the number of beads

Line 2:	a string of N characters, each of which is r, b, or w

##### SAMPLE INPUT (file beads.in)
```
29
wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
```

##### OUTPUT FORMAT
A single line containing the maximum of number of beads that can be collected 
from the supplied necklace.

##### SAMPLE OUTPUT (file beads.out)
```
11
```

##### OUTPUT EXPLANATION
Consider two copies of the beads (kind of like being able to runaround the 
ends). The string of 11 is marked.
```
            Two necklace copies joined here
                             v
wwwbbrwrbrbrrbrbrwrwwrbwrwrrb|wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
                       ******|*****
                       rrrrrb|bbbbb  <-- assignments
                   5xr .....#|#####  6xb

                        5+6 = 11 total
```    

### Analysis:
This program can be solved by using [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming){:target="_blank" rel="noopener"}. 
The basic process is as follows. Split the necklace and lay it straight. 
Start from the left end and count the number of beads that are of the same 
color as the current bead. We can leverage the count from the results of the
preceding bead to compute the count for the current bead. Continue right until
you reach the end. Then start at the right end and repeat the same process 
going left. The result will be the maximum sum of the left and right counts at 
a given location. You need to be aware that the count may exceed N if all the 
beads are of the same color (e.g.'r' and/or 'w', or 'b' and/or 'w'), in which 
case the result is N.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class beads {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("beads.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("beads.out")))) {

            final int RED = 0;
            final int BLUE = 1;

            // number of beads in necklace
            final int N = Integer.parseInt(f.readLine());

            // string of N characters representing the necklace, each of which is r, b, or w
            final String necklace = f.readLine();

            // concatenate two copies of the necklace to simulate a circular necklace in linear format
            final String s = necklace + necklace;
            final char[] a = s.toCharArray();

            // compute left side using dynamic programming
            int[][] left = new int[N * 2][2];
            left[0][RED] = 0;       // populate seed value for RED
            left[0][BLUE] = 0;      // populate seed value for BLUE
            for (int i = 1; i < N * 2; i++) {
                if (a[i - 1] == 'r') {
                    left[i][RED] = left[i - 1][RED] + 1;
                    left[i][BLUE] = 0;
                } else if (a[i - 1] == 'b') {
                    left[i][BLUE] = left[i - 1][BLUE] + 1;
                    left[i][RED] = 0;
                } else {
                    // increment for both since 'w' can be either RED or BLUE
                    left[i][RED] = left[i - 1][RED] + 1;
                    left[i][BLUE] = left[i - 1][BLUE] + 1;
                }
            }

            // compute right side using dynamic programming
            int[][] right = new int[(N * 2) + 1][2];
            right[2 * N][RED] = 0;  // populate seed value for RED
            right[2 * N][BLUE] = 0; // populate seed value for BLUE
            for (int i = (N * 2) - 1; i >= 0; i--) {
                if (a[i] == 'r') {
                    right[i][RED] = right[i + 1][RED] + 1;
                    right[i][BLUE] = 0;
                } else if (a[i] == 'b') {
                    right[i][BLUE] = right[i + 1][BLUE] + 1;
                    right[i][RED] = 0;
                } else {
                    // increment for both since 'w' can be either RED or BLUE
                    right[i][RED] = right[i + 1][RED] + 1;
                    right[i][BLUE] = right[i + 1][BLUE] + 1;
                }
            }

            // find the maximum value for (left[i] + right[i])
            int max = 0;
            for (int i = 0; i < N * 2; i++) {
                max = Math.max(max,
                        Math.max(left[i][RED], left[i][BLUE])
                                + Math.max(right[i][RED], right[i][BLUE]));
            }

            // check for case where necklace is of a single color, in which
            // case the max length would be N
            max = Math.min(N, max);

            // output
            out.println(max);
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/beads.java){:target="_blank" rel="noopener"}

### Links:
* [Subset Sums](/usaco/subset)
* [Cow Pedigrees](/usaco/nowcows)
* [Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming](/blog/find-nth-fibonacci-number-dynamic-programming.html)
