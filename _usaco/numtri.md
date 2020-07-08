---
layout: default
title: "Number Triangles"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.6.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Consider the number triangle shown below. Write a program that calculates the 
highest sum of numbers that can be passed on a route that starts at the top 
and ends somewhere on the base. Each step can go either diagonally down to the 
left or diagonally down to the right.
```
          7

        3   8

      8   1   0

    2   7   4   4

  4   5   2   6   5
```
In the sample above, the route from 7 to 3 to 8 to 7 to 5 produces the highest 
sum: 30. 

##### PROGRAM NAME: numtri

##### INPUT FORMAT
The first line contains R (1 <= R <= 1000), the number of rows. Each subsequent 
line contains the integers for that particular row of the triangle. All the 
supplied integers are non-negative and no larger than 100.

##### SAMPLE INPUT (file numtri.in)
```
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5
```

##### OUTPUT FORMAT
A single line containing the largest sum using the traversal specified.

##### SAMPLE OUTPUT (file numtri.out)
```
30
```

### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class numtri {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("numtri.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("numtri.out")))) {

            // number of rows
            final int R = Integer.parseInt(f.readLine());

            // construct triangle; [0][0] is upper left corner
            final int[][] triangle = new int[R][R];
            for (int i = 0; i < R; i++) {
                final String line = f.readLine();
                final String[] tokens = line.split(" ");
                for (int j = 0; j < tokens.length; j++) {
                    triangle[i][j] = Integer.parseInt(tokens[j]);
                }
            }

            // initialize values in sums to -1
            final int[][] sums = new int[R][R];
            for (int row = 0; row < sums.length; row++) {
                for (int col = 0; col < sums.length; col++) {
                    sums[row][col] = -1;
                }
            }

            // find max sum
            sum(0, 0, triangle, sums);

            // output result
            out.println(sums[0][0]);
        }
    }

    private static int sum(int row, int col, int[][] triangle, int[][] sums) {
        // special case for last row of triangle
        if (row == (triangle.length - 1)) {
            // the sum is just the value in the triangle
            setSum(row, col, triangle[row][col], sums);
            return sums[row][col];
        }

        // check if sum is already calculated for this node at [row][col];
        // if so (e.g. not -1), then we already know the maximum sum for this
        // node; there is no need to calculate it again;
        //
        // this is an important optimization to eliminate duplicate calculations
        // that have already been performed; otherwise it will take too long to
        // compute when you have a large number of rows
        final int nodeSum = sums[row][col];
        if (nodeSum != -1) {
            return nodeSum;
        }

        // keep traversing down the rows to find the maximum sum
        setSum(row, col, triangle[row][col] + sum(row + 1, col, triangle, sums), sums);
        setSum(row, col, triangle[row][col] + sum(row + 1, col + 1, triangle, sums), sums);

        // return the maximum sum for this node at [row][col]
        return sums[row][col];
    }

    private static void setSum(int row, int col, int sum, int[][] sums) {
        // only save the largest sum
        if (sum > sums[row][col]) {
            sums[row][col] = sum;
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/numtri.java){:target="_blank" rel="noopener"}
