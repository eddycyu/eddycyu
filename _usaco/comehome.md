---
layout: default
title: "Bessie Come Home"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.4.4
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
It's dinner time, and the cows are out in their separate pastures. Farmer John 
rings the bell so they will start walking to the barn. Your job is to figure 
out which one cow gets to the barn first (the supplied test data will always 
have exactly one fastest cow).

Between milkings, each cow is located in her own pasture, though some pastures 
have no cows in them. Each pasture is connected by a path to one or more other 
pastures (potentially including itself). Sometimes, two (potentially self-same) 
pastures are connected by more than one path. One or more of the pastures has a 
path to the barn. Thus, all cows have a path to the barn and they always know 
the shortest path. Of course, cows can go either direction on a path and they 
all walk at the same speed.

The pastures are labeled 'a'..'z' and 'A'..'Y'. One cow is in each pasture 
labeled with a capital letter. No cow is in a pasture labeled with a lower case 
letter. The barn's label is 'Z'; no cows are in the barn, though.

##### PROGRAM NAME: comehome

##### INPUT FORMAT

Line 1:           | Integer P (1 <= P <= 10000) the number of paths that interconnect the pastures (and the barn)
------------------|-------------------
Line&nbsp;2..P+1: | Space separated, two letters and an integer: the names of the interconnected pastures/barn and the distance between them (1 <= distance <= 1000)

##### SAMPLE INPUT (file comehome.in)
```
5
A d 6
B d 3
C e 9
d Z 8
e Z 3
```

##### OUTPUT FORMAT
A single line containing two items: the capital letter name of the pasture of 
the cow that arrives first back at the barn, the length of the path followed 
by that cow.

##### SAMPLE OUTPUT (file comehome.out)
```
B 11
```

### Analysis:
To solve for the shortest path, we can use the 
[Floyd-Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm){:target="_blank" rel="noopener"}
by treating the pastures (i.e. vertices) and distances between the pastures 
(i.e. weights) as a weighted graph with positive weights. This algorithm will 
find the shortest path between all pairs of vertices.

### Solution:
```java
public class comehome {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("comehome.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("comehome.out")))) {

            // number of paths
            final int numberOfPaths = Integer.parseInt(f.readLine());

            // distance between pastures [A-Z,a-z][A-Z,a-z] where Z = barn
            final int[][] distance = new int[52][52];

            // initially set large distance between pastures;
            // 100,000,000 is safe to use since all path distances
            // are <= 1,000 and there are <= 100,000 paths
            for (int i = 0; i < distance.length; i++) {
                for (int j = 0; j < distance.length; j++) {
                    distance[i][j] = 100000000;
                }
            }
            // set distance for self looping paths to zero
            for (int i = 0; i < 26; i++) {
                distance[i][i] = 0;
            }

            // set known distance between pastures
            for (int i = 0; i < numberOfPaths; i++) {
                final String[] tokens = f.readLine().split(" ");
                final int pastureA = charToInt(tokens[0].charAt(0));
                final int pastureB = charToInt(tokens[1].charAt(0));
                final int dist = Integer.parseInt(tokens[2]);
                // keep shortest distance
                if (distance[pastureA][pastureB] > dist) {
                    distance[pastureA][pastureB] = dist;
                    distance[pastureB][pastureA] = dist;
                }
            }

            // use Floyd-Warshall algorithm to solve all pair shortest path
            for (int k = 0; k < distance.length; k++) {
                for (int i = 0; i < distance.length; i++) {
                    for (int j = 0; j < distance.length; j++) {
                        if (distance[i][j] >= (distance[i][k] + distance[k][j])) {
                            distance[i][j] = distance[i][k] + distance[k][j];
                        }
                    }
                }
            }

            // output pasture with cow that arrives back first and length of shortest path
            int pasture = Integer.MAX_VALUE;
            int shortestDistance = -1;
            for (int i = 0; i < 25; i++) {
                int dist = distance[25][i];
                if ((shortestDistance == -1) || (dist < shortestDistance)) {
                    pasture = i;
                    shortestDistance = dist;
                }
            }
            out.println(intToChar(pasture) + " " + shortestDistance);
        }
    }

    private static int charToInt(char character) {
        final int number = character - 65;
        if ((number >= 0) && (number <= 25)) {
            // uppercase (pasture with cow) where Z is barn
            // values 0 (A) to 25 (Z)
            return number;
        } else if ((number >= 32) && (number <= 57)) {
            // lowercase (pasture with no cow)
            // values 26 (a) to 51 (z)
            return number - 6;
        } else {
            return -1; // invalid
        }
    }

    private static char intToChar(int number) {
        if ((number >= 0) && (number <= 25)) {
            // uppercase (pasture with cow) where Z is barn
            // values 0 (A) to 25 (Z)
            return (char) (number + 65);
        } else if ((number >= 26) && (number <= 51)) {
            // lowercase (pasture with no cow)
            // values 26 (a) to 51 (z)
            return (char) (number + 65 + 6);
        } else {
            return ' '; // invalid
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/comehome.java){:target="_blank" rel="noopener"}
