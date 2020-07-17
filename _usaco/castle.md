---
layout: default
title: "The Castle (IOI'94 - Day 1)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, recursion]
order: 2.1.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
In a stroke of luck almost beyond imagination, Farmer John was sent a ticket to 
the Irish Sweepstakes (really a lottery) for his birthday. This ticket turned 
out to have only the winning number for the lottery! Farmer John won a fabulous 
castle in the Irish countryside.

Bragging rights being what they are in Wisconsin, Farmer John wished to tell 
his cows all about the castle. He wanted to know how many rooms it has and how 
big the largest room was. In fact, he wants to take out a single wall to make 
an even bigger room.

Your task is to help Farmer John know the exact room count and sizes.

The castle floorplan is divided into M (wide) by N (1 <=M,N<=50) square 
modules. Each such module can have between zero and four walls. Castles 
always have walls on their "outer edges" to keep out the wind and rain.

Consider this annotated floorplan of a castle:
```
     1   2   3   4   5   6   7
   #############################
 1 #   |   #   |   #   |   |   #
   #####---#####---#---#####---#   
 2 #   #   |   #   #   #   #   #
   #---#####---#####---#####---#
 3 #   |   |   #   #   #   #   #   
   #---#########---#####---#---#
 4 # ->#   |   |   |   |   #   #   
   ############################# 

#  = Wall     -,|  = No wall
-> = Points to the wall to remove to make the largest possible new room
```
By way of example, this castle sits on a 7 x 4 base. A "room" includes any set 
of connected "squares" in the floor plan. This floorplan contains five rooms 
(whose sizes are 9, 7, 3, 1, and 8 in no particular order).

Removing the wall marked by the arrow merges a pair of rooms to make the 
largest possible room that can be made by removing a single wall.

The castle always has at least two rooms and always has a wall that can be 
removed.

##### PROGRAM NAME: castle

##### INPUT FORMAT
The map is stored in the form of numbers, one number for each module ("room"),
M numbers on each of N lines to describe the floorplan. The input order 
corresponds to the numbering in the example diagram above.

Each module descriptive-number tells which of the four walls exist and is the 
sum of up to four integers:

* 1: wall to the west
* 2: wall to the north
* 4: wall to the east
* 8: wall to the south

Inner walls are defined twice; a wall to the south in module 1,1 is also 
indicated as a wall to the north in module 2,1.

----------|---------
Line 1:	  | Two space-separated integers: M and N
----------|---------
Line 2..: | M x N integers, several per line.

##### SAMPLE INPUT (file castle.in)
```
7 4
11 6 11 6 3 10 6
7 9 6 13 5 15 5
1 10 12 7 13 7 5
13 11 10 8 10 12 13
```

##### OUTPUT FORMAT
The output contains several lines:

--------|--------
Line 1: | The number of rooms the castle has.
Line 2: | The size of the largest room
Line 3: | The size of the largest room creatable by removing one wall
Line 4: | The single wall to remove to make the largest room possible

Choose the optimal wall to remove from the set of optimal walls by choosing the 
module farthest to the west (and then, if still tied, farthest to the south). 
If still tied, choose 'N' before 'E'. Name that wall by naming the module that 
borders it on either the west or south, along with a direction of N or E giving 
the location of the wall with respect to the module.

##### SAMPLE OUTPUT (file castle.out)
```
5
9
16
4 1 E
```

##### INPUT DETAILS
First, the map is partitioned like below. Note that walls not on the outside 
borders are doubled:
```
     1    2    3    4    5    6    7
   ####|####|####|####|####|####|#####
 1 #   |   #|#   |   #|#   |    |    #
   ####|   #|####|   #|#   |####|    #
  -----|----|----|----|----|----|-----
   ####|#   |####|#  #|#  #|####|#   #
 2 #  #|#   |   #|#  #|#  #|#  #|#   #
   #  #|####|   #|####|#  #|####|#   #
  -----|----|----|----|----|----|-----
   #   |####|   #|####|#  #|####|#   #
 3 #   |    |   #|#  #|#  #|#  #|#   #
   #   |####|####|#  #|####|#  #|#   #
  -----|----|----|----|----|----|-----
   #  #|####|####|    |####|   #|#   #
 4 #  #|#   |    |    |    |   #|#   #
   ####|####|####|####|####|####|#####
```
Let's talk about the squares with a (row, column) notation such that the lower 
right corner is denoted (4, 7).

The input will have four lines, each with 7 numbers. Each number describes one 
'room'. >Walls further toward the top are 'north', towards the bottom are 
'south', towards the left are 'west', and towards the right are 'east'.

Consider square (1,1) which has three walls: north, south, and west. To encode 
those three walls, we consult the chart:

 * 1: wall to the west
 * 2: wall to the north
 * 4: wall to the east
 * 8: wall to the south

and sum the numbers for north (2), south (8), and west (1). 2 + 8 + 1 = 11, so 
this room is encoded as 11.

The next room to the right (1,2) has walls on the north (2) and east (4) and 
thus is encoded as 2 + 4 = 6.

The next room to the right (1,3) is the same as (1,1) and thus encodes as 11.

Room (1,4) is the same as (1,2) and thus encodes as 6.

Room (1,5) has rooms on the west (1) and north (2) and thus encodes as 1 + 2 = 3.

Room (1,6) has rooms on the north (2) and south (8) and thus encodes as 2 + 8 = 10.

Room (1,7) is the same as room (1,2) and thus encodes as 6.

This same method continues for rooms (2,1) through (4,7).

### Analysis:
Since we are going to use the westernmost and then the southernmost squares,
we only need to consider removing the walls on the north and east.

### Solution:
This problem can be solved using recursion.

First use recursion to assign a room number to each square. Then loop through 
each square to compute the size of each room number. Once computed, use 
recursion to remove walls (north and east) one at a time to see if a larger
room is created, and keep track of which removal resulted in the creation of 
the largest room.

```java
public class castle {

    static final int WEST = 0x1;
    static final int NORTH = 0x2;
    static final int EAST = 0x4;
    static final int SOUTH = 0x8;

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("castle.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("castle.out")))) {

            // read castle dimensions
            final String[] dimensions = f.readLine().split(" ");
            final int maxColumns = Integer.parseInt(dimensions[0]);
            final int maxRows = Integer.parseInt(dimensions[1]);

            // read castle squares into array
            final int[][] squares = new int[maxRows][maxColumns];
            for (int row = 0; row < maxRows; row++) {
                final String[] tokens = f.readLine().split(" ");
                for (int col = 0; col < maxColumns; col++) {
                    squares[row][col] = Integer.parseInt(tokens[col]);
                }
            }

            // assign room numbers to each square
            final int[][] roomNumbers = new int[maxRows][maxColumns];
            final int numberOfRooms = assignAllRoomNumbers(roomNumbers, maxRows, maxColumns, squares);

            // output the number of rooms the castle has
            out.println(numberOfRooms);

            // compute room sizes and find the largest room size
            final int[] roomSizes = computeRoomSizes(roomNumbers, numberOfRooms, maxRows, maxColumns);
            final int maxRoomSize = getLargestRoomSize(roomSizes);

            // output the size of the largest room
            out.println(maxRoomSize);

            // remove walls (from westernmost and southernmost squares)
            int afterRemoveMaxRoomSize = maxRoomSize;
            int afterRemoveRow = maxRows;
            int afterRemoveCol = maxColumns;
            int afterRemoveWall = 0;
            for (int col = 0; col < maxColumns; col++) {
                for (int row = 0; row < maxRows; row++) {
                    final int[] result = removeWall(row, col, maxColumns, squares, roomNumbers, roomSizes);
                    final int lastSize = result[0];
                    final int lastRow = result[1] + 1;
                    final int lastCol = result[2] + 1;
                    if (lastSize > afterRemoveMaxRoomSize) {
                        afterRemoveMaxRoomSize = lastSize;
                        afterRemoveRow = lastRow;
                        afterRemoveCol = lastCol;
                        afterRemoveWall = result[3];
                    } else if ((lastSize == afterRemoveMaxRoomSize) && (lastCol <= afterRemoveCol)) {
                        afterRemoveRow = lastRow;
                        afterRemoveCol = lastCol;
                        afterRemoveWall = result[3];
                    }
                }
            }

            // output size of the largest room created by removing one wall
            out.println(afterRemoveMaxRoomSize);

            // output the single wall to remove to make the largest room possible
            out.println(afterRemoveRow + " " + afterRemoveCol + " " + (afterRemoveWall == NORTH ? "N" : "E"));
        }
    }

    private static boolean hasWestWall(int number) {
        return (number & WEST) == WEST;
    }

    private static boolean hasNorthWall(int number) {
        return (number & NORTH) == NORTH;
    }

    private static boolean hasEastWall(int number) {
        return (number & EAST) == EAST;
    }

    private static boolean hasSouthWall(int number) {
        return (number & SOUTH) == SOUTH;
    }

    private static int assignAllRoomNumbers(int[][] roomNumbers, int maxRows, int maxColumns, int[][] squares) {
        // first mark all rooms with room number = -1
        for (int row = 0; row < maxRows; row++) {
            for (int col = 0; col < maxColumns; col++) {
                roomNumbers[row][col] = -1;
            }
        }

        // then assign room numbers to each square
        int nextRoomNumber = 1;
        for (int row = 0; row < maxRows; row++) {
            for (int col = 0; col < maxColumns; col++) {
                nextRoomNumber = assignRoomNumber(roomNumbers, row, col, nextRoomNumber, maxRows, maxColumns, squares);
            }
        }

        // return the number of rooms
        return nextRoomNumber - 1;
    }

    private static int assignRoomNumber(int[][] roomNumbers, int row, int col, int nextRoomNumber,
                                        int maxRows, int maxColumns, int[][] squares) {
        // assign room number and increment counter for next room number
        if (roomNumbers[row][col] == -1) {
            roomNumbers[row][col] = nextRoomNumber++;
        }

        // check west wall
        if ((col != 0) && !hasWestWall(squares[row][col]) && (roomNumbers[row][col - 1] == -1)) {
            roomNumbers[row][col - 1] = roomNumbers[row][col];
            assignRoomNumber(roomNumbers, row, col - 1, nextRoomNumber, maxRows, maxColumns, squares);
        }

        // check south wall
        if ((row != maxRows - 1) && !hasSouthWall(squares[row][col]) && (roomNumbers[row + 1][col] == -1)) {
            roomNumbers[row + 1][col] = roomNumbers[row][col];
            assignRoomNumber(roomNumbers, row + 1, col, nextRoomNumber, maxRows, maxColumns, squares);
        }

        // check north wall
        if ((row != 0) && !hasNorthWall(squares[row][col]) && (roomNumbers[row - 1][col] == -1)) {
            roomNumbers[row - 1][col] = roomNumbers[row][col];
            assignRoomNumber(roomNumbers, row - 1, col, nextRoomNumber, maxRows, maxColumns, squares);
        }

        // check east wall
        if ((col != maxColumns - 1) && !hasEastWall(squares[row][col]) && (roomNumbers[row][col + 1] == -1)) {
            roomNumbers[row][col + 1] = roomNumbers[row][col];
            assignRoomNumber(roomNumbers, row, col + 1, nextRoomNumber, maxRows, maxColumns, squares);
        }

        return nextRoomNumber;
    }

    private static int[] computeRoomSizes(int[][] roomNumbers, int numberOfRooms, int maxRows, int maxColumns) {
        final int[] roomSizes = new int[numberOfRooms];
        for (int row = 0; row < maxRows; row++) {
            for (int col = 0; col < maxColumns; col++) {
                final int roomNumber = roomNumbers[row][col];
                roomSizes[roomNumber - 1] += 1;
            }
        }
        return roomSizes;
    }

    private static int getLargestRoomSize(int[] roomSizes) {
        int maxRoomSize = 0;
        for (int roomSize : roomSizes) {
            maxRoomSize = Math.max(maxRoomSize, roomSize);
        }
        return maxRoomSize;
    }

    public static int[] removeWall(int row, int col, int maxColumns, int[][] squares,
                                   int[][] roomNumbers, int[] roomSizes) {
        final int originalSquare = squares[row][col];
        final int currentRoomNumber = roomNumbers[row][col];

        int maxRoomSizeFromNorthWall = roomSizes[currentRoomNumber - 1];
        if ((row != 0) && hasNorthWall(squares[row][col])) {
            // remove north wall from current square
            final int modifiedSquare = originalSquare ^ NORTH;
            squares[row][col] = modifiedSquare;

            // remove south wall from adjoining square
            final int originalAdjoiningSquare = squares[row - 1][col];
            final int modifiedAdjoiningSquare = originalAdjoiningSquare ^ SOUTH;
            squares[row - 1][col] = modifiedAdjoiningSquare;

            // compute newly combined room size (if combined distinct rooms)
            final int adjoiningRoomNumber = roomNumbers[row - 1][col];
            if (currentRoomNumber != adjoiningRoomNumber) {
                maxRoomSizeFromNorthWall += roomSizes[adjoiningRoomNumber - 1];
            }

            // restore original walls
            squares[row][col] = originalSquare;
            squares[row - 1][col] = originalAdjoiningSquare;
        }

        int maxRoomSizeFromEastWall = roomSizes[currentRoomNumber - 1];
        if ((col != maxColumns - 1) && hasEastWall(squares[row][col])) {
            // remove east wall from current square
            final int modifiedSquare = originalSquare ^ EAST;
            squares[row][col] = modifiedSquare;

            // remove west wall for adjoining square
            final int originalAdjoiningSquare = squares[row][col + 1];
            final int modifiedAdjoiningSquare = originalAdjoiningSquare ^ WEST;
            squares[row][col + 1] = modifiedAdjoiningSquare;

            // compute newly combined room size (if combined distinct rooms)
            final int adjoiningRoomNumber = roomNumbers[row][col + 1];
            if (currentRoomNumber != adjoiningRoomNumber) {
                maxRoomSizeFromEastWall += roomSizes[adjoiningRoomNumber - 1];
            }

            // restore original walls
            squares[row][col] = originalSquare;
            squares[row][col + 1] = originalAdjoiningSquare;
        }

        // return the removal that resulted in the largest combined rooms
        if (maxRoomSizeFromNorthWall >= maxRoomSizeFromEastWall) {
            return new int[]{maxRoomSizeFromNorthWall, row, col, NORTH};
        } else {
            return new int[]{maxRoomSizeFromEastWall, row, col, EAST};
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/pprime.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion)
