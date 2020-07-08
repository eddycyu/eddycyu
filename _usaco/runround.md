---
layout: default
title: "Runaround Numbers"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.2.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Runaround numbers are integers with unique digits, none of which is zero 
(e.g., 81362) that also have an interesting property, exemplified by this 
demonstration:

* If you start at the left digit (8 in our number) and count that number of 
  digits to the right (wrapping back to the first digit when no digits on the 
  right are available), you'll end up at a new digit (a number which does not 
  end up at a new digit is not a Runaround Number). Consider: 8 1 3 6 2 which 
  cycles through eight digits: 1 3 6 2 8 1 3 6 so the next digit is 6.
* Repeat this cycle (this time for the six counts designed by the `6') and you
  should end on a new digit: 2 8 1 3 6 2, namely 2.
* Repeat again (two digits this time): 8 1
* Continue again (one digit this time): 3
* One more time: 6 2 8 and you have ended up back where you started, after 
  touching each digit once. If you don't end up back where you started after 
  touching each digit once, your number is not a Runaround number.

Given a number M (that has anywhere from 1 through 9 digits), find and print 
the next runaround number higher than M, which will always fit into an unsigned 
long integer for the given test data.

##### PROGRAM NAME: runround

##### INPUT FORMAT
A single line with a single integer, M

##### SAMPLE INPUT (file runround.in)
```
81361
```

##### OUTPUT FORMAT
A single line containing the next runaround number higher than the input value, M.

##### SAMPLE OUTPUT (file runround.out)
```
81362
```
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class runround {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("runround.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("runround.out")))) {

            final int M = Integer.parseInt(f.readLine());
            for (int m = M + 1; m < 999999999; m++) {
                final String number = Integer.toString(m);
                if (isRunround(number)) {
                    out.println(m);
                    break;
                }
            }

        }
    }

    private static boolean isRunround(String number) {
        boolean[] seen = new boolean[10];
        boolean[] visitedIndex = new boolean[number.length() + 1];
        int index = 0;
        int increment = Character.getNumericValue(number.charAt(index));
        int count = 0;

        do {
            // 0 is an invalid increment
            if (increment == 0) {
                return false;
            }
            // check if number wraps around to itself
            if (increment % number.length() == 0) {
                return false;
            }
            // check if increment is repeated
            if (seen[increment]) {
                return false;
            }
            seen[increment] = true;

            // advance index
            index = (index + increment) % (number.length());

            // check if next index has been visited before
            if (visitedIndex[index]) {
                return false;
            }
            visitedIndex[index] = true;
            increment = Character.getNumericValue(number.charAt(index));
        } while (++count < number.length());

        return true;
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/runround.java){:target="_blank" rel="noopener"}
