---
layout: default
title: "Find The Shortest Path (Dijkstra's Algorithm)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, graph, shortest path, greedy]
author: Eddy Yu
published: true
---

This recipe shows how to find the shortest path between a single source vertex
and all other vertices in a weighted graph. This implementation uses 
[Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm){:target="_blank" rel="noopener"},
also known as Dijkstra's Shortest Path First (SPF) algorithm.

This algorithm works for both directed and undirected weighted graphs, but the
weights must be positive.

Note: The Floyd-Marshall algorithm is for finding the shortest path between all
vertices while Dijkstra's algorithm and the Bellman-Ford algorithm are for 
finding the shortest path between a single vertex and all other vertices (or 
the shortest path between two given nodes).

### Problem:
Given a weighted graph, find the shortest path between a source vertex and all
other (reachable) vertices.

### Solution:
```java
public class Dijkstra {

    private static void shortestPath(int[][] graph, int source) {
        final int V = graph.length;
        final boolean[] visitedVertex = new boolean[V];
        final int[] distance = new int[V];

        // assign infinity path distance to each vertex
        Arrays.fill(distance, Integer.MAX_VALUE);

        // assign zero path distance to source (self loop)
        distance[source] = 0;

        for (int i = 0; i < V; i++) {
            // find next unvisited vertex with minimum path distance
            final int fromV = findVertexMinDistance(distance, visitedVertex);
            if (fromV == -1) {
                break; // no more unvisited vertices that can be reached
            }

            // mark this vertex as processed
            visitedVertex[fromV] = true;

            // update the path distance for each adjacent vertex if the new
            // distance is less than the existing distance to adjacent vertex
            for (int toV = 0; toV < V; toV++) {
                if (!visitedVertex[toV]
                        && (graph[fromV][toV] != 0)
                        && (distance[fromV] + graph[fromV][toV] < distance[toV])) {
                    distance[toV] = distance[fromV] + graph[fromV][toV];
                }
            }
        }

        // output shortest paths from source vertex to other vertices
        printShortestPathFromSource(source, distance);
    }

    // find unvisited vertex with the minimum path distance
    private static int findVertexMinDistance(int[] distance, boolean[] visitedVertex) {
        int minDistance = Integer.MAX_VALUE;
        int minDistanceVertex = -1;
        for (int i = 0; i < distance.length; i++) {
            if (!visitedVertex[i]
                    && (distance[i] != Integer.MAX_VALUE)
                    && (distance[i] < minDistance)) {
                minDistance = distance[i];
                minDistanceVertex = i;
            }
        }
        return minDistanceVertex;
    }

    private static void printShortestPathFromSource(int source, int[] distance) {
        for (int i = 0; i < distance.length; i++) {
            if (distance[i] != Integer.MAX_VALUE) {
                System.out.println(String.format("Distance from vertex[%d] to vertex[%d] is %d: ",
                        source, i, distance[i]));
            }
        }
    }

    public static void main(String[] args) {
        /*
         * number of vertices = 9
         * V0 <-> V2 = 1
         * V0 <-> V3 = 2
         * V1 <-> V2 = 2
         * V1 <-> V5 = 3
         * V2 <-> V3 = 1
         * V2 <-> V4 = 3
         * V3 <-> V6 = 1
         * V4 <-> V5 = 2
         * V5 <-> V6 = 1
         * V7 --> V6 = 4
         * V8 --> V7 = 2
         */
        final int[][] graph = new int[][]{
                {0, 0, 1, 2, 0, 0, 0, 0, 0},
                {0, 0, 2, 0, 0, 3, 0, 0, 0},
                {1, 2, 0, 1, 3, 0, 0, 0, 0},
                {2, 0, 1, 0, 0, 0, 1, 0, 0},
                {0, 0, 3, 0, 0, 2, 0, 0, 0},
                {0, 3, 0, 0, 2, 0, 1, 0, 0},
                {0, 0, 0, 1, 0, 1, 0, 0, 0},
                {0, 0, 0, 0, 0, 0, 4, 0, 0},
                {0, 0, 0, 0, 0, 0, 0, 2, 0}};
        shortestPath(graph, 0);
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/graph/Dijkstra.java){:target="_blank" rel="noopener"}

### Solution Notes:
Space Complexity: O(V)

Time Complexity: O(E log V)


### Links:
* [Find The Shortest Path (Floyd-Warshall Algorithm)](/blog/find-shortest-path-floyd-warshall)
* [Find The Shortest Path (Bellman-Ford Algorithm)](/blog/find-shortest-path-bellman-ford)