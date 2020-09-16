---
layout: default
title: "Cow Tours"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.4.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Farmer John has a number of pastures on his farm. Cow paths connect some pastures with certain other pastures, forming 
a field. But, at the present time, you can find at least two pastures that cannot be connected by any sequence of cow 
paths, thus partitioning Farmer John's farm into multiple fields.

Farmer John would like add a single a cow path between one pair of pastures using the constraints below.

A field's _diameter_ is defined to be the largest distance of all the shortest walks between any pair of pastures in 
the field. Consider the field below with five pastures, located at the points shown, and cow paths marked by lines:
```
                15,15   20,15
                  D       E
                  *-------*
                  |     _/|
                  |   _/  |
                  | _/    |
                  |/      |
         *--------*-------*
         A        B       C
        10,10    15,10   20,10
```
The _diameter_ of this field is approximately 12.07106, since the longest of the set of shortest paths between pairs of 
pastures is the path from A to E (which includes the point set {A,B,E}). No other pair of pastures in this field is 
farther apart when connected by an optimal sequence of cow paths.

Suppose an additional field on the same plane as above is connected by cow paths as follows:
```
                         *F 30,15
                        / 
                      _/  
                    _/    
                   /      
                  *------*
                  G      H
                 25,10  30,10
```
Given just these two fields on his farm, Farmer John would add a cow path between a point in each of these two fields 
(namely point sets {A,B,C,D,E} and {F,G,H}) so that the joined set of pastures {A,B,C,D,E,F,G,H} has the smallest 
possible diameter.

Note that cow paths do not connect just because they cross each other; they only connect at listed points.

The input contains the pastures, their locations, and a symmetric "adjacency" matrix that tells whether pastures are 
connected by cow paths. Pastures are not considered to be connected to themselves. Here's one annotated adjacency list 
for the pasture {A,B,C,D,E,F,G,H} as shown above:
```
                A B C D E F G H
              A 0 1 0 0 0 0 0 0
              B 1 0 1 1 1 0 0 0
              C 0 1 0 0 1 0 0 0
              D 0 1 0 0 1 0 0 0
              E 0 1 1 1 0 0 0 0
              F 0 0 0 0 0 0 1 0
              G 0 0 0 0 0 1 0 1
              H 0 0 0 0 0 0 1 0
```
Other equivalent adjacency lists might permute the rows and columns by using some order other than alphabetical to show 
the point connections. The input data contains no names for the points.

The input will contain at least two pastures that are not connected by any sequence of cow paths.

Find a way to connect exactly two pastures in the input with a cow path so that the new combined field has the smallest 
possible diameter of any possible pair of connected pastures. Output that smallest possible diameter.

##### PROGRAM NAME: cowtour

##### INPUT FORMAT

Line 1:	       | An integer, N (1 <= N <= 150), the number of pastures
---------------|-------------------
Line 2-N+1:    | Two integers, X and Y (0 <= X ,Y<= 100000), that denote that X,Y grid location of the pastures; all input pastures are unique.
Line N+2-2*N+1 | Lines, each containing N digits (0 or 1) that represent the adjacency matrix as described above, where the rows' and columns' indices are in order of the points just listed.

##### SAMPLE INPUT (file cowtour.in)
```
8
10 10
15 10
20 10
15 15
20 15
30 15
25 10
30 10
01000000
10111000
01001000
01001000
01110000
00000010
00000101
00000010
```

##### OUTPUT FORMAT
The output consists of a single line with the diameter of the newly joined pastures. Print the answer to exactly six 
decimal places. Do not perform any special rounding on your output.

##### SAMPLE OUTPUT (file cowtour.out)
```
22.071068
```

##### OUTPUT EXPLANATION
After trying all possible connection pairs, connecting C to G yields the minimum diameter.

### ANALYSIS
Given that this problem involves finding the shortest path between all pairs of pastures, we know that we will need to
use a shortest path algorithm (e.g. Floyd-Warshall Algorithm). This will require that we:
* compute the distance of each path connecting adjacent pastures
* traverse through the adjacency matrix and group the pastures into fields

At this point, we may be tempted to try joining pasture pairs across fields and using the Floyd-Warshall algorithm 
to see which pair produces the smallest possible diameter. This will work when there are only a few pastures, but once
the number of pastures increases, we will quickly find ourselves exceeding the allowable time limit.

Instead, we need to realize the following observation. When joining two fields via pasture A (field 1) and pasture B
(field 2), we can find the diameter (longest shortest path) of the newly combined field joined at those two pastures 
with the following equation:
```
longest shortest path of field joined at pasture A+B =
  (longest shortest path from pasture A to any other pasture in field 1) 
  + (longest shortest path from pasture B to any other pasture in field 2)
  + (distance between pasture A and pasture B) 
```
This holds true as long as there doesn't exist a longer shortest path in either field 1 or field 2. Thus, the longest
shortest path of the combined field is:
```
longest shortest path of combined field =
  max(longest shortest path of field joined at pasture A+B, longest shortest path in either field 1 or field 2)
```
If we use this observation, we only need to run the Floyd-Warshall algorithm once.

### Solution:
```java
class cowtour {

    final static double INF = Double.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("cowtour.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("cowtour.out")))) {
            // number of pastures
            final int N = Integer.parseInt(f.readLine());

            // x-y coordinates for each pasture
            final int[] X = new int[N];
            final int[] Y = new int[N];
            for (int n = 0; n < N; n++) {
                String[] tokens = f.readLine().split(" ");
                X[n] = Integer.parseInt(tokens[0]);
                Y[n] = Integer.parseInt(tokens[1]);
            }

            // adjacency matrix ([0][0] = upper left corner)
            boolean[][] adj = new boolean[N][N];
            for (int y = 0; y < N; y++) {
                final String[] tokens = f.readLine().split("");
                for (int x = 0; x < N; x++) {
                    if (tokens[x].equals("1")) {
                        adj[x][y] = true;
                        adj[y][x] = true;
                    }
                }
            }

            // compute distance for each path (edges) between adjacent pastures (vertices)
            double[][] graph = new double[N][N]; // value = distance between two adjacent pastures
            for (int y = 0; y < N; y++) {
                for (int x = 0; x < N; x++) {
                    graph[x][y] = INF;
                }
            }
            for (int y = 1; y < N; y++) {
                for (int x = 0; x < y; x++) {
                    if (adj[x][y]) {
                        final double distance = getDistance(x, y, X, Y);
                        graph[x][y] = distance;
                        graph[y][x] = distance;
                    }
                }
            }

            // group the pastures into distinct fields
            final List<Set<Integer>> fields = getFields(N, adj);

            // use modified Floyd-Marshall algorithm to find shortest path between all pair of pastures
            final double[][] shortestPaths = findShortestPaths(graph);

            // keep track of the largest diameter before combining any fields
            final double maxDiameterBeforeCombine = getDiameter(shortestPaths);

            /*
             * At this point, we know:
             * (1) all distinct fields
             * (2) the shortest paths for each pair of pastures
             * (3) the max diameter (before combining any fields)
             *
             * We can now connect two pastures from two different fields and compute the combined path:
             *
             *   combinedDistance =
             *      [the longest shortest path from pasture A in field 1 to any other pasture in field 1]
             *      + [the longest shortest path from pasture B in field 2 to any other pasture in field 2]
             *      + [the distance from pasture A to pasture B]
             *
             * The solution will be the larger of:
             * (1) min(combinedDistance)
             * (2) max diameter before combining any fields
             */

            // iterate through all the fields and compute
            double minDiameter = INF;
            for (int i = 0; i < fields.size() - 1; i++) {
                for (int j = 1; j < fields.size(); j++) {
                    if (i != j) {
                        final Set<Integer> field1 = fields.get(i);
                        final Set<Integer> field2 = fields.get(j);
                        for (int y : field1) {
                            final double pasture1Distance = getLongestShortestPathForPasture(y, shortestPaths);
                            for (int x : field2) {
                                // compute combined distance if pasture(x) and pasture(y) are joined
                                final double pasture2Distance = getLongestShortestPathForPasture(x, shortestPaths);
                                final double newPathDistance = getDistance(x, y, X, Y);
                                final double combinedDistance = pasture1Distance + pasture2Distance + newPathDistance;
                                minDiameter = Math.min(minDiameter, combinedDistance);
                            }
                        }
                    }
                }
            }
            minDiameter = Math.max(minDiameter, maxDiameterBeforeCombine);

            // output result
            out.printf("%.6f\n", minDiameter);
        }
    }

    private static double getDistance(int pasture1, int pasture2, int[] X, int[] Y) {
        final int x1 = X[pasture1];
        final int y1 = Y[pasture1];
        final int x2 = X[pasture2];
        final int y2 = Y[pasture2];
        return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
    }

    private static List<Set<Integer>> getFields(int N, boolean[][] adj) {
        // group the pastures into distinct fields
        final List<Set<Integer>> fields = new ArrayList<>();
        final Set<Integer> assigned = new HashSet<>();
        Set<Integer> field = new HashSet<>();
        for (int n = 0; n < N; n++) {
            findConnectedPastures(n, N, adj, field);
            if (field.isEmpty() && !assigned.contains(n)) {
                // n is the only pasture in the field
                field.add(n);
            }
            if (!field.isEmpty()) {
                fields.add(field);
                assigned.addAll(field);
                field = new HashSet<>();
            }
        }
        return fields;
    }

    private static void findConnectedPastures(int n, int N, boolean[][] adj, Set<Integer> field) {
        // find all pastures connected to pasture(n) by some path
        for (int i = 0; i < N; i++) {
            if ((i != n) && adj[n][i]) {
                if (!field.contains(i)) {
                    field.add(n);
                    field.add(i);
                    findConnectedPastures(i, N, adj, field);
                }
                // remove pastures from adjacency matrix
                adj[n][i] = false;
                adj[i][n] = false;
            }
        }
    }

    private static double[][] findShortestPaths(double[][] graph) {
        // make copy of graph for storing distance
        final int V = graph.length;
        final double[][] distance = new double[V][V];
        for (int i = 0; i < V; i++) {
            System.arraycopy(graph[i], 0, distance[i], 0, V);
        }

        // compute shortest path for all pairs of vertices; assume no cycles
        for (int k = 0; k < V; k++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {
                    if ((i != j)
                            && (distance[i][k] != INF) && (distance[k][j] != INF)
                            && (distance[i][k] + distance[k][j]) < distance[i][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                    }
                }
            }
        }
        return distance;
    }

    private static double getDiameter(double[][] shortestPaths) {
        // diameter is the longest distance of all the shortest paths
        double diameter = 0;
        for (int i = 1; i < shortestPaths.length; i++) {
            for (int j = 0; j < i; j++) {
                if (shortestPaths[i][j] != INF) {
                    diameter = Math.max(diameter, shortestPaths[i][j]);
                }
            }
        }
        return diameter;
    }

    private static double getLongestShortestPathForPasture(int pasture, double[][] shortestPaths) {
        double longest = 0;
        for (int i = 0; i < shortestPaths.length; i++) {
            if (shortestPaths[pasture][i] != INF) {
                longest = Math.max(longest, shortestPaths[pasture][i]);
            }
        }
        return longest;
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/cowtour.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Shortest Path (Floyd-Warshall Algorithm)](/blog/find-shortest-path-floyd-warshall)
* [Find The Shortest Path (Bellman-Ford Algorithm)](/blog/find-shortest-path-bellman-ford)