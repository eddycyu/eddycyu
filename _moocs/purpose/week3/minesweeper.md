---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 3: Arrays - Minesweeper"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.3.4
---

Minesweeper is a 1960s era video game played on an _m-by-n_ grid of cells. The 
goal is to deduce which cells contain hidden mines using clues about the 
number of mines in neighboring cells. Write a program Minesweeper.java that 
takes three integer command-line arguments _m_, _n_, and _k_ and prints an 
_m-by-n_ grid of cells with _k_ mines, using asterisks for mines and integers 
for the neighboring mine counts (with two space characters between each 
cell). To do so,

* Generate an _m-by-n_ grid of cells, with exactly _k_ of the _mn_ cells 
  containing mines, uniformly at random.
* For each cell not containing a mine, count the number of neighboring mines 
  (above, below, left, right, or diagonal). 

```
~/Desktop/arrays> java Minesweeper 9 9 10
0  1  *  1  0  0  0  1  *  
1  3  2  2  0  0  0  1  1  
*  2  *  1  0  0  1  1  1  
1  2  2  2  1  0  1  *  1  
0  1  2  *  1  0  1  1  1  
1  2  *  3  3  1  1  0  0  
1  *  3  *  2  *  1  0  0  
1  1  2  1  2  1  1  0  0  
0  0  0  0  0  0  0  0  0 
```

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/arrays/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class Minesweeper {

    public static void main(String[] args) {
        final int m = Integer.parseInt(args[0]); // row
        final int n = Integer.parseInt(args[1]); // col
        final int k = Integer.parseInt(args[2]);

        final int[][] grid = new int[m][n];
        final int[] mineRow = new int[k];
        final int[] mineCol = new int[k];

        // generate mines
        final double[] randomValues = new double[2];
        int row;
        int col;
        int mineCount = 0;
        while (mineCount < k) {
            // required for passing the tests
            for (int i = 0; i < 2; i++) {
                randomValues[i] = Math.random();
            }
            row = (int) (randomValues[0] * m);
            col = (int) (randomValues[1] * n);
            if (grid[row][col] >= 0) {
                grid[row][col] = Integer.MIN_VALUE;
                mineRow[mineCount] = row;
                mineCol[mineCount] = col;
                mineCount++;
            }
        }

        // sum for neighboring cells next to mines
        final int maxRowIndex = m - 1;
        final int maxColIndex = n - 1;
        for (int i = 0; i < k; i++) {
            row = mineRow[i];
            col = mineCol[i];
            if (row > 0) {
                grid[row - 1][col]++; // up
                if (col > 0) {
                    grid[row - 1][col - 1]++; // upper left
                }
                if (col < maxColIndex) {
                    grid[row - 1][col + 1]++; // upper right
                }
            }
            if (row < maxRowIndex) {
                grid[row + 1][col]++; // down
                if (col > 0) {
                    grid[row + 1][col - 1]++; // lower left
                }
                if (col < maxColIndex) {
                    grid[row + 1][col + 1]++; // lower right
                }
            }
            if (col > 0) {
                grid[row][col - 1]++; // left
            }
            if (col < maxColIndex) {
                grid[row][col + 1]++; // right
            }
        }

        // output
        for (row = 0; row < m; row++) {
            for (col = 0; col < n; col++) {
                if (grid[row][col] < 0) {
                    System.out.print("*  ");
                } else {
                    System.out.print(grid[row][col] + "  ");
                }
            }
            System.out.println();
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/Minesweeper.java){:target="_blank" rel="noopener"}
