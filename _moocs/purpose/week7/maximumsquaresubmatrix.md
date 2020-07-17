---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 7: Performance - Maximum Square Submatrix"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.7.3
---

Given an _n-by-n_ matrix of 0s and 1s, find a contiguous square submatrix of 
maximum size that contains only 1s. To do so, organize your program 
according to the following public API:

```java
public class MaximumSquareSubmatrix {

    // Returns the size of the largest contiguous square submatrix
    // of a[][] containing only 1s.
    public static int size(int[][] a)

    // Reads an n-by-n matrix of 0s and 1s from standard input
    // and prints the size of the largest contiguous square submatrix
    // containing only 1s.
    public static void main(String[] args)
}
```

Here is some more information about the required behavior:

* _Size._ The size of a square submatrix is its number of rows (or columns). You 
  may assume that argument to the _size()_ method is a square matrix containing 
  only 0s and 1s.
* _Contiguous._ The square submatrix must be contiguous — the row indicies must 
  be consecutive and the column indices must be consecutive.
* _Performance._ The _size()_ method should take time proportional to _n_ <sup>2</sup> 
  in the worst case. Significant partial credit for solutions that take time 
  proportional to _n_ <sup>3</sup> or _n_ <sup>4</sup>.
* _Input format._ Standard input will contain a positive integer _n_, followed 
  by _n_ lines, with each line containing _n_ 0s and 1s, separated by whitespace.

Here are a few sample executions:

```
~/Desktop/performance> cat square6.txt
6
0  1  1  0  1  1
1  1  0  1  0  1
0  1  1  1  0  1
1  1  1  1  1  0
1  1  1  1  1  1
0  0  0  0  1  1

~/Desktop/performance> java MaximumSquareSubmatrix < input6.txt
3

~/Desktop/performance> cat square7.txt
7
0  1  1  0  1  1  1
1  1  0  1  1  1  1
0  1  1  1  1  1  1
1  1  1  1  1  0  1
1  1  1  1  1  1  0
1  1  1  1  0  1  1
1  1  1  1  0  1  1

~/Desktop/performance> java MaximumSquareSubmatrix < square7.txt
4
```

The maximum square submatrix problem is related to problems that arise in 
databases, image processing, and maximum likelihood estimation. It is also 
a popular technical job interview question.

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/performance/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class MaximumSquareSubmatrix {

    private static void labelCells(int[][] a, int row, int col, int size) {
        for (int r = row; r < (row + size); r++) {
            for (int c = col; c < col + size; c++) {
                if (a[r][c] < size) {
                    a[r][c] = size;
                }
            }
        }
    }

    private static int contig(int[][] a, int row, int col, int size, int maxRow, int maxCol) {
        if (a[row][col] == 0) {
            return size; // no increase, return previous size
        }

        /**
         * example:
         *
         * 011?0
         * 011?0
         * 0???0
         *
         * size = 2
         * check for (2*size+1) counts
         */

        int count = 0;
        for (int r = 0; r < size + 1; r++) {
            // check rows (size+1) at fixed column [vertical direction]
            if (a[row + r][col + size] > 0) {
                count++;
            }
        }
        for (int c = 0; c < size; c++) {
            // check columns (size) at fixed row [horizontal direction]
            if (a[row + size][col + c] > 0) {
                count++;
            }
        }
        if (count == (2 * size + 1)) {
            // found next larger square
            size++;
            if ((row + size - 1 < maxRow) && (col + size - 1 < maxCol)) {
                // try same row, next column
                return contig(a, row, col, size, maxRow, maxCol);
            } else {
                // no more columns to try; return current size
                return size;
            }
        } else {
            return size; // no increase in size, return previous size
        }
    }

    // Returns the size of the largest contiguous square submatrix
    // of a[][] containing only 1s.
    public static int size(int[][] a) {
        // make copy of a (to pass 'no mutate' test)
        int maxSize = 0;
        final int maxRowCol = a[0].length - 1;
        final int[][] a2 = new int[maxRowCol + 1][maxRowCol + 1];
        for (int row = 0; row < a2[0].length; row++) {
            for (int col = 0; col < a2[0].length; col++) {
                a2[row][col] = a[row][col];
                maxSize = Math.max(maxSize, a2[row][col]);
            }
        }

        // matrix row/col must be 2 or larger; else just return value in single cell matrix
        if (a2[0].length < 2) {
            return a2[0][0];
        }

        // sum up largest contiguous square submatrix (row by row)
        for (int row = 0; row < maxRowCol; row++) {
            for (int col = 0; col < maxRowCol; col++) {
                if (a2[row][col] > 0) {
                    final int size = contig(a, row, col, 1, maxRowCol, maxRowCol);
                    // mark submatrix with size
                    labelCells(a2, row, col, size);
                    // remember max size
                    maxSize = Math.max(maxSize, size);
                }
            }
        }

        // return largest
        return maxSize;
    }

    // Reads an n-by-n matrix of 0s and 1s from standard input
    // and prints the size of the largest contiguous square submatrix
    // containing only 1s.
    public static void main(String[] args) {
        final int n = StdIn.readInt();
        final int[][] a = new int[n][n];
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                a[row][col] = StdIn.readInt();
            }
        }
        StdOut.println(size(a));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/MaximumSquareSubmatrix.java){:target="_blank" rel="noopener"}
