---
layout: default
title: "Barn Repair"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.4.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
It was a dark and stormy night that ripped the roof and gates off the stalls 
that hold Farmer John's cows. Happily, many of the cows were on vacation, so 
the barn was not completely full.

The cows spend the night in stalls that are arranged adjacent to each other 
in a long line. Some stalls have cows in them; some do not. All stalls are 
the same width.

Farmer John must quickly erect new boards in front of the stalls, since the 
doors were lost. His new lumber supplier will supply him boards of any 
length he wishes, but the supplier can only deliver a small number of total 
boards. Farmer John wishes to minimize the total length of the boards he 
must purchase.

Given M (1 <= M <= 50), the maximum number of boards that can be purchased; 
S (1 <= S <= 200), the total number of stalls; C (1 <= C <= S) the number 
of cows in the stalls, and the C occupied stall numbers 
(1 <= stall_number <= S), calculate the minimum number of stalls that must 
be blocked in order to block all the stalls that have cows in them.

Print your answer as the total number of stalls blocked.

##### PROGRAM NAME: barn1

##### INPUT FORMAT

Line 1:      | M, S, and C (space separated)
-------------|---------------------
Lines 2-C+1: | Each line contains one integer, the number of an occupied stall.


##### SAMPLE INPUT (file barn1.in)
```
4 50 18
3
4
6
8
14
15
16
17
21
25
26
27
30
31
40
41
42
43
```

##### OUTPUT FORMAT
A single line with one integer that represents the total number of stalls blocked.

##### SAMPLE OUTPUT (file barn1.out)
```
25
```
One minimum arrangement is one board covering stalls 3-8, one covering 14-21, one 
covering 25-31, and one covering 40-43.

### Analysis:
We can use a [greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm){:target="_blank" rel="noopener"} 
to solve this problem.
    
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

class barn1 {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("barn1.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("barn1.out")))) {

            String line = f.readLine();
            final String[] tokens = line.split(" ");
            final int maxBoards = Integer.parseInt(tokens[0]);
            final int totalStalls = Integer.parseInt(tokens[1]);
            final int occupiedStalls = Integer.parseInt(tokens[2]);

            // mark stalls with 'true' if occupied
            final boolean[] blockedStalls = new boolean[totalStalls];
            final List<Integer> occupiedStallsList = new ArrayList<>();
            for (int i = 0; i < occupiedStalls; i++) {
                line = f.readLine();
                final int stallNumber = Integer.parseInt(line);
                blockedStalls[stallNumber - 1] = true;
                occupiedStallsList.add(stallNumber);
            }

            // sort the occupied stalls list (in case it is not provided in sorted order)
            Collections.sort(occupiedStallsList);

            // record the gaps
            final List<Gap> gaps = new ArrayList<>();
            int previousOccupiedStallNumber = 0;
            for (Integer currentOccupiedStallNumber : occupiedStallsList) {
                if (currentOccupiedStallNumber - previousOccupiedStallNumber > 1) {
                    gaps.add(
                            new Gap(previousOccupiedStallNumber + 1,
                                    currentOccupiedStallNumber - 1,
                                    previousOccupiedStallNumber + 1 == 1));
                }
                previousOccupiedStallNumber = currentOccupiedStallNumber;
            }

            // record right-most outer gap (if any)
            if (previousOccupiedStallNumber < totalStalls) {
                gaps.add(new Gap(previousOccupiedStallNumber + 1, totalStalls, true));
            }

            // sort the gaps by length
            Collections.sort(gaps);

            // calculate boards required
            int boardsRequired = 0;
            if (blockedStalls[0] && blockedStalls[blockedStalls.length - 1]) {
                // no exterior gaps (all interior gaps)
                boardsRequired = gaps.size() + 1;
            } else if ((blockedStalls[0] && !blockedStalls[blockedStalls.length - 1])
                    || (!blockedStalls[0] && blockedStalls[blockedStalls.length - 1])) {
                // only one exterior gap
                boardsRequired = gaps.size();
            } else {
                // both exterior gaps
                boardsRequired = gaps.size() - 1;
            }

            // reduce gaps to reduce boards
            while (boardsRequired > maxBoards) {
                // get shortest gap
                final Gap gap = gaps.remove(0);
                // mark stalls
                for (int i = gap.startingStallNumber - 1; i < gap.endingStallNumber; i++) {
                    blockedStalls[i] = true;
                }
                boardsRequired--;
            }

            // calculate total number of stalls blocked
            int totalBlocked = 0;
            for (boolean blockedStall : blockedStalls) {
                if (blockedStall) {
                    totalBlocked++;
                }
            }

            // output result
            out.println(totalBlocked);
        }
    }

    /**
     * Identifies a gap between empty stalls. For example:
     * <p>
     * Gep between stalls 3 and 5 = Gap(4, 4).
     * Gap between stalls 6 and 9 = Gap(7, 8)
     */
    static class Gap implements Comparable<Gap> {
        int startingStallNumber = 0;
        int endingStallNumber = 0;
        int length = 0;
        boolean outerGap = false;

        private Gap() {
            super();
        }

        Gap(int startingStallNumber, int endingStallNumber, boolean outerGap) {
            this();
            this.startingStallNumber = startingStallNumber;
            this.endingStallNumber = endingStallNumber;
            this.outerGap = outerGap;
            this.length = endingStallNumber - startingStallNumber + 1;
        }

        @Override
        public int compareTo(Gap other) {
            // sort outer gaps to the end
            if (this.outerGap && !other.outerGap) {
                return (1);
            } else if (!this.outerGap && other.outerGap) {
                return (-1);
            } else {
                return (this.length - other.length);
            }
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/barn1.java){:target="_blank" rel="noopener"}

### Links:
* [Palindromic Squares](/usaco/palsquare)
* [Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming](/blog/find-nth-fibonacci-number-dynamic-programming)
