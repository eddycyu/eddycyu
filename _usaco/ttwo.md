---
layout: default
title: "The Tamworth Two"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.4.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
A pair of cows is loose somewhere in the forest. Farmer John is lending his 
expertise to their capture. Your task is to model their behavior.

The chase takes place on a 10 by 10 planar grid. Squares can be empty or they 
can contain:
* an obstacle,
* the cows (who always travel together), or
* Farmer John.

The cows and Farmer John can occupy the same square (when they 'meet') but 
neither the cows nor Farmer John can share a square with an obstacle.

Here is a sample grid:
```
*...*.....
......*...
...*...*..
..........
...*.F....
*.....*...
...*......
..C......*
...*.*....
.*.*......
```

Each square is represented as follows:
* . Empty square
* \* Obstacle
* C Cows
* F Farmer

The cows wander around the grid in a fixed way. Each minute, they either move 
forward or rotate. Normally, they move one square in the direction they are 
facing. If there is an obstacle in the way or they would leave the board by 
walking 'forward', then they spend the entire minute rotating 90 degrees 
clockwise.

Farmer John, wise in the ways of cows, moves in exactly the same way.

The farmer and the cows can be considered to move simultaneously during each 
minute. If the farmer and the cows pass each other while moving, they are not 
considered to have met. The chase ends when Farmer John and the cows occupy the 
same square at the end of a minute.

Read a ten-line grid that represents the initial state of the cows, Farmer John, 
and obstacles. Each of the ten lines contains exactly ten characters using the 
coding above. There is guaranteed to be only one farmer and one pair of cows. 
The cows and Farmer John will not initially be on the same square.

Calculate the number of minutes until the cows and Farmer John meet. Assume both 
the cows and farmer begin the simulation facing in the 'north' direction. Print 
0 if they will never meet.

##### PROGRAM NAME: ttwo

##### INPUT FORMAT

------------|-------------------
Lines 1-10: | Ten lines of ten characters each, as explained above


##### SAMPLE INPUT (file ttwo.in)
```
*...*.....
......*...
...*...*..
..........
...*.F....
*.....*...
...*......
..C......*
...*.*....
.*.*......
```

##### OUTPUT FORMAT
A single line with the integer number of minutes until Farmer John and the cows 
meet. Print 0 if they will never meet.

##### SAMPLE OUTPUT (file ttwo.out)
```
49
```

### Solution:
```java
public class ttwo {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("ttwo.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("ttwo.out")))) {

            // forest is 10x10 grid
            // [0][0] = upper left corner
            final String[][] grid = new String[10][10];

            // starting direction (0 = north, 1 = east, 2 = south, 3 = west)
            int farmerDirection = 0;
            int cowsDirection = 0;

            // location index
            int farmerI = -1;
            int farmerJ = -1;
            int cowsI = -1;
            int cowsJ = -1;

            // read grid (remember starting index locations for Farmer John and cows)
            for (int i = 0; i < 10; i++) {
                String[] tokens = f.readLine().split("");
                for (int j = 0; j < 10; j++) {
                    grid[i][j] = tokens[j];
                    if (grid[i][j].equals("F")) {
                        farmerI = i;
                        farmerJ = j;
                    } else if (grid[i][j].equals("C")) {
                        cowsI = i;
                        cowsJ = j;
                    }
                }
            }

            // used for keeping track of previously visited location and direction (for detecting cycles)
            final boolean[][] farmerVisited = new boolean[10][10];
            final int[][] farmerVisitedDirection = new int[10][10];
            final boolean[][] cowsVisited = new boolean[10][10];
            final int[][] cowsVisitedDirection = new int[10][10];

            // record visit and direction at starting location
            farmerVisited[farmerI][farmerJ] = true;
            farmerVisitedDirection[farmerI][farmerJ] = farmerDirection;
            cowsVisited[cowsI][cowsJ] = true;
            cowsVisitedDirection[cowsI][cowsJ] = cowsDirection;

            // solve
            int minutes = 0;
            while (true) {
                minutes++;

                /*
                 * process Farmer John
                 */

                // facing north on upper row or facing east on right column
                // or facing south on bottom row or facing west on left column;
                // cannot move forward; just rotate
                if (((farmerDirection == 0) && (farmerI == 0))
                        || ((farmerDirection == 1) && (farmerJ == 9))
                        || ((farmerDirection == 2) && (farmerI == 9))
                        || ((farmerDirection == 3) && (farmerJ == 0))) {
                    farmerDirection = (farmerDirection + 1) % 4;
                }
                // facing north with obstacle in the way or facing east with obstacle in the way
                // or facing south with obstacle in the way or facing west with obstacle in the way;
                // cannot move forward; just rotate
                else if (((farmerDirection == 0) && (grid[farmerI - 1][farmerJ].equals("*")))
                        || ((farmerDirection == 1) && (grid[farmerI][farmerJ + 1].equals("*")))
                        || ((farmerDirection == 2) && (grid[farmerI + 1][farmerJ].equals("*")))
                        || ((farmerDirection == 3) && (grid[farmerI][farmerJ - 1].equals("*")))) {
                    farmerDirection = (farmerDirection + 1) % 4;
                }
                // can move forward
                else {
                    if (farmerDirection == 0) {
                        farmerI -= 1;
                    } else if (farmerDirection == 1) {
                        farmerJ += 1;
                    } else if (farmerDirection == 2) {
                        farmerI += 1;
                    } else if (farmerDirection == 3) {
                        farmerJ -= 1;
                    }
                }

                /*
                 * process cows
                 */
                
                // facing north on upper row or facing east on right column
                // or facing south on bottom row or facing west on left column;
                // cannot move forward; just rotate
                if (((cowsDirection == 0) && (cowsI == 0))
                        || ((cowsDirection == 1) && (cowsJ == 9))
                        || ((cowsDirection == 2) && (cowsI == 9))
                        || ((cowsDirection == 3) && (cowsJ == 0))) {
                    cowsDirection = (cowsDirection + 1) % 4;
                }
                // facing north with obstacle in the way or facing east with obstacle in the way
                // or facing south with obstacle in the way or facing west with obstacle in the way;
                // cannot move forward; just rotate
                else if (((cowsDirection == 0) && (grid[cowsI - 1][cowsJ].equals("*")))
                        || ((cowsDirection == 1) && (grid[cowsI][cowsJ + 1].equals("*")))
                        || ((cowsDirection == 2) && (grid[cowsI + 1][cowsJ].equals("*")))
                        || ((cowsDirection == 3) && (grid[cowsI][cowsJ - 1].equals("*")))) {
                    cowsDirection = (cowsDirection + 1) % 4;
                }
                // can move forward
                else {
                    if (cowsDirection == 0) {
                        cowsI -= 1;
                    } else if (cowsDirection == 1) {
                        cowsJ += 1;
                    } else if (cowsDirection == 2) {
                        cowsI += 1;
                    } else if (cowsDirection == 3) {
                        cowsJ -= 1;
                    }
                }

                // check if cycle
                if ((farmerVisited[farmerI][farmerJ] && (farmerVisitedDirection[farmerI][farmerJ] == farmerDirection))
                        && (cowsVisited[cowsI][cowsJ] && (cowsVisitedDirection[cowsI][cowsJ] == cowsDirection))) {
                    out.println(0);
                    break;
                } else {
                    // else record visit at new location and direction
                    farmerVisited[farmerI][farmerJ] = true;
                    farmerVisitedDirection[farmerI][farmerJ] = farmerDirection;
                    cowsVisited[cowsI][cowsJ] = true;
                    cowsVisitedDirection[cowsI][cowsJ] = cowsDirection;
                }

                // check if same location
                if ((farmerI == cowsI) && (farmerJ == cowsJ)) {
                    out.println(minutes);
                    break;  // terminate loop
                }
            }
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/ttwo.java){:target="_blank" rel="noopener"}
