---
layout: default
title: "Find The Shortest Path (Floyd-Warshall Algorithm)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, graph, shortest path]
author: Eddy Yu
published: true
---

This recipe shows how to find the shortest path between all pairs of vertices
in a weighted graph. This implementation uses the 
[Floyd-Warshall algorithm](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm){:target="_blank" rel="noopener"}.

This algorithm works for both directed and undirected weighted graphs. The
weights can be positive or negative. It will report if there is a negative
cycle (where the sum of the edges in a cycle is negative), but will not
compute the shortest path in such case.

Note: The Floyd-Marshall algorithm is for finding the shortest path between all
vertices while Dijkstra's algorithm and the Bellman-Ford algorithm are for 
finding the shortest path between a single vertex and all other vertices (or 
the shortest path between two given nodes).

### Problem:
Given a weighted graph, find the shortest path between all pairs of vertices.

### Solution:
```java
public class FloydWarshall {

    final static int INF = Integer.MAX_VALUE;

    private static void shortestPath(int[][] graph) {
        // make copy of graph for storing distance
        final int V = graph.length;
        final int[][] distance = new int[V][V];
        for (int i = 0; i < V; i++) {
            System.arraycopy(graph[i], 0, distance[i], 0, V);
        }

        // compute shortest path
        for (int k = 0; k < V; k++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {
                    if ((distance[i][k] != INF) && (distance[k][j] != INF)
                            && (distance[i][k] + distance[k][j]) < distance[i][j]) {
                        distance[i][j] = distance[i][k] + distance[k][j];
                    }
                }
                // check for negative cycle
                if (distance[i][i] < 0) {
                    System.out.println("graph contains negative cycle");
                    return;
                }
            }
        }

        // output shortest paths for all pairs of vertices
        printShortestPathFromSource(distance);
    }

    private static void printShortestPathFromSource(int[][] distance) {
        for (int i = 0; i < distance.length; i++) {
            for (int j = 0; j < distance.length; j++) {
                if (distance[i][j] == INF) {
                    System.out.print("INF ");
                } else {
                    System.out.print(distance[i][j] + "  ");
                }
            }
            System.out.println();
        }

        for (int i = 0; i < distance.length; i++) {
            for (int j = 0; j < distance.length; j++) {
                if (distance[i][j] != INF) {
                    System.out.println(String.format("Distance from vertex[%d] to vertex[%d] is %d: ",
                            i, j, distance[i][j]));
                }
            }
        }
    }

    public static void main(String[] args) {
         /*
         * number of vertices = 4
         * V0->V1 = 3
         * V1->V0 = 2
         * V0->V3 = 5
         * V1->V3 = 4
         * V2->V1 = 1
         * V2->V2 = 0
         * V3->V2 = 2
         * V3->V3 = 0
         */
        final int[][] graph = {
                {0, 3, INF, 5},
                {2, 0, INF, 4},
                {INF, 1, 0, INF},
                {INF, INF, 2, 0}};
        System.out.println("graph with no negative cycle:");
        shortestPath(graph);

        /*
         * number of vertices = 4
         * V0->V0 = 0
         * V0->V1 = 1
         * V1->V1 = 0
         * V1->V2 = -1
         * V2->V2 = 0
         * V2->V3 = -1
         * V3->V0 = -1
         * V3->V3 = 0
         *
         * negative cycle: 0->1->2>3->0
         */
        final int[][] graphNegativeCycle = {
                {0, 1, INF, INF},
                {INF, 0, -1, INF},
                {INF, INF, 0, -1},
                {-1, INF, INF, 0}};
        System.out.println();
        System.out.println("graph with negative cycle:");
        shortestPath(graphNegativeCycle);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/graph/FloydWarshall.java){:target="_blank" rel="noopener"}

### Solution Notes:
Space Complexity: O(V^2)

Time Complexity: O(V^3)

### Links:
* [Find The Shortest Path (Dijkstra's Algorithm)](/blog/find-shortest-path-dijkstra)
* [Find The Shortest Path (Bellman-Ford Algorithm)](/blog/find-shortest-path-bellman-ford)