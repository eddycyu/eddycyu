---
layout: default
title: "Sorting A Three-Valued Sequence (IOI'96 - Day 2)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.1.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Sorting is one of the most frequently performed computational tasks. Consider 
the special sorting problem in which the records to be sorted have at most 
three different key values. This happens for instance when we sort medalists 
of a competition according to medal value, that is, gold medalists come first, 
followed by silver, and bronze medalists come last.

In this task the possible key values are the integers 1, 2 and 3. The required 
sorting order is non-decreasing. However, sorting has to be accomplished by a 
sequence of exchange operations. An exchange operation, defined by two position 
numbers p and q, exchanges the elements in positions p and q.

You are given a sequence of key values. Write a program that computes the 
minimal number of exchange operations that are necessary to make the sequence 
sorted.

##### PROGRAM NAME: sort3

##### INPUT FORMAT

Line 1:      | N (1 <= N <= 1000), the number of records to be sorted
-------------|---------------------
Lines 2-N+1: | A single integer from the set {1, 2, 3}


##### SAMPLE INPUT (file sort3.in)
```
9
2
2
1
3
3
3
2
3
1
```

##### OUTPUT FORMAT
A single line containing the number of exchanges required.

##### SAMPLE OUTPUT (file sort3.out)
```
4
```

### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class sort3 {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("sort3.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("sort3.out")))) {

            final int N = Integer.parseInt(f.readLine());
            final int[] records = new int[N];
            for (int i = 0; i < N; i++) {
                records[i] = Integer.parseInt(f.readLine());
            }

            final int numberOf1 = countNumberOfRecordsOf(records, 1, 0, N - 1);
            final int numberOf2 = countNumberOfRecordsOf(records, 2, 0, N - 1);
            final int numberOf3 = N - numberOf1 - numberOf2;

            final int index1Start = 0;
            final int index1End = numberOf1 - 1;
            final int index2Start = numberOf1;
            final int index2End = numberOf1 + numberOf2 - 1;
            final int index3Start = numberOf1 + numberOf2;
            final int index3End = N - 1;

            // determine how many of x are in bucket y (num2In1, num3In1, num1In2, ...)
            int num2In1 = countNumberOfRecordsOf(records, 2, index1Start, index1End);
            int num3In1 = countNumberOfRecordsOf(records, 3, index1Start, index1End);

            int num1In2 = countNumberOfRecordsOf(records, 1, index2Start, index2End);
            int num3In2 = countNumberOfRecordsOf(records, 3, index2Start, index2End);

            int num1In3 = countNumberOfRecordsOf(records, 1, index3Start, index3End);
            int num2In3 = countNumberOfRecordsOf(records, 2, index3Start, index3End);

            // distribute the numbers to their respective buckets
            int exchanges = 0;
            while ((num2In1 + num3In1 + num1In2 + num3In2 + num1In3 + num2In3) > 0) {

                if (num1In2 <= num2In1) {
                    exchanges += num1In2;
                    num2In1 -= num1In2;
                    num1In2 = 0;
                } else {
                    exchanges += num2In1;
                    num1In2 -= num2In1;
                    num2In1 = 0;
                }

                if (num1In3 <= num3In1) {
                    exchanges += num1In3;
                    num3In1 -= num1In3;
                    num1In3 = 0;
                } else {
                    exchanges += num3In1;
                    num1In3 -= num3In1;
                    num3In1 = 0;
                }

                if (num2In3 <= num3In2) {
                    exchanges += num2In3;
                    num3In2 -= num2In3;
                    num2In3 = 0;
                } else {
                    exchanges += num3In2;
                    num2In3 -= num3In2;
                    num3In2 = 0;
                }

                if (((num2In1 == num3In2) && (num2In1 == num1In3))
                        && ((num2In3 == 0) && (num3In1 == 0) && (num1In2 == 0))) {
                    exchanges += (2 * num2In1);
                    num2In1 = num3In2 = num1In3 = 0;

                } else if (((num3In1 == num1In2) && (num3In1 == num2In3))
                        && ((num3In2 == 0) && (num1In3 == 0) && (num2In1 == 0))) {
                    exchanges += (2 * num3In1);
                    num3In1 = num1In2 = num2In3 = 0;
                }
            }

            out.println(exchanges);
        }
    }

    private static int countNumberOfRecordsOf(int[] records, int number, int indexStart, int indexEnd) {
        int count = 0;
        for (int i = indexStart; i <= indexEnd; i++) {
            if (records[i] == number) {
                count++;
            }
        }
        return count;
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/sort3.java){:target="_blank" rel="noopener"}
