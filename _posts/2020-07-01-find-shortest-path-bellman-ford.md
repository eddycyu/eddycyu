---
layout: default
title: "Find The Shortest Path (Bellman-Ford Algorithm)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, graph, shortest path, dynamic programming]
author: Eddy Yu
published: true
---

This recipe shows how to find the shortest path between a single source vertex
and all other vertices in a weighted graph. This implementation uses the
[Bellman-Ford algorithm](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm){:target="_blank" rel="noopener"}.

This algorithm works for both directed and undirected weighted graphs, and
unlike Dijkstra's algorithm, it works with both positive and negative
weights. It will report if there is a negative cycle (where the sum of the
edges in a cycle is negative), but will not compute the shortest path in
such case. It performs slower than Dijkstra's algorithm, but is well suited
for implementation in distributed systems.

Note: The Floyd-Marshall algorithm is for finding the shortest path between all
vertices while Dijkstra's algorithm and the Bellman-Ford algorithm are for 
finding the shortest path between a single vertex and all other vertices (or 
the shortest path between two given nodes).

### Problem:
Given a weighted graph, find the shortest path between all pairs of vertices.

### Solution:
```java
public class BellmanFord {

    private static void shortestPath(int[][] graph, int source) {
        final int V = graph.length;
        final int[] distance = new int[V];
        final int[] predecessor = new int[V];

        // assign infinity path distance to each vertex
        Arrays.fill(distance, Integer.MAX_VALUE);

        // assign zero path distance to source (self loop)
        distance[source] = 0;

        // assign -1 predecessor to each vertex
        Arrays.fill(predecessor, -1);

        // get edges
        final List<Edge> edges = getEdges(graph);

        // relax all edges repeatedly
        int src, dest;
        for (int i = 0; i < V; i++) {
            for (Edge edge : edges) {
                src = edge.source;
                dest = edge.destination;
                // update the path distance if shorter
                if ((distance[src] != Integer.MAX_VALUE)
                        && (distance[src] + graph[src][dest] < distance[dest])) {
                    distance[dest] = distance[src] + graph[src][dest];
                    predecessor[dest] = src;
                }
            }
        }

        // check for negative cycle
        for (Edge edge : edges) {
            src = edge.source;
            dest = edge.destination;
            if ((distance[src] != Integer.MAX_VALUE)
                    && (distance[src] + graph[src][dest] < distance[dest])) {
                System.out.println("graph contains negative cycle");
                return;
            }
        }

        // output shortest paths from source vertex to other vertices
        printShortestPathFromSource(source, distance, predecessor);
    }

    private static void printShortestPathFromSource(int source, int[] distance, int[] predecessor) {
        for (int i = 0; i < distance.length; i++) {
            if (distance[i] != Integer.MAX_VALUE) {
                System.out.printf("Distance from vertex[%d] to vertex[%d] is %d: ",
                        source, i, distance[i]);
                final Stack<Integer> path = getPath(source, i, predecessor);
                while (!path.isEmpty()) {
                    System.out.print(path.pop());
                    if (!path.isEmpty()) {
                        System.out.print("->");
                    }
                }
                System.out.println();
            }
        }
    }

    private static Stack<Integer> getPath(int source, int destination, int[] predecessor) {
        final Stack<Integer> path = new Stack<>();

        // check for self looping path
        if (predecessor[destination] == -1) {
            path.push(source);
            path.push(source);
            return path;
        }
        // push path to stack
        for (int vertex = destination; predecessor[vertex] != -1; vertex = predecessor[vertex]) {
            path.push(vertex);
        }
        // push source to stack
        path.push(source);
        return path;
    }

    private static List<Edge> getEdges(int[][] graph) {
        final int V = graph.length;
        final List<Edge> edges = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (graph[i][j] != 0) {
                    edges.add(new Edge(i, j));
                }
            }
        }
        return edges;
    }

    static class Edge {
        private final int source;
        private final int destination;

        Edge(int source, int destination) {
            super();
            this.source = source;
            this.destination = destination;
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
                {0, 0, -1, 2, 0, 0, 0, 0, 0},
                {0, 0, 2, 0, 0, 3, 0, 0, 0},
                {1, 2, 0, 1, 3, 0, 0, 0, 0},
                {2, 0, 1, 0, 0, 0, 1, 0, 0},
                {0, 0, 3, 0, 0, 2, 0, 0, 0},
                {0, 3, 0, 0, 2, 0, 1, 0, 0},
                {0, 0, 0, 1, 0, 1, 0, 0, 0},
                {0, 0, 0, 0, 0, 0, 4, 0, 0},
                {0, 0, 0, 0, 0, 0, 0, 2, 0}};
        System.out.println("graph with no negative cycle:");
        shortestPath(graph, 0);

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
         *
         * negative cycle: 0->2->3->0
         */
        final int[][] graphNegativeCycle = new int[][]{
                {0, 0, -1, 2, 0, 0, 0, 0, 0},
                {0, 0, 2, 0, 0, 3, 0, 0, 0},
                {1, 2, 0, 1, 3, 0, 0, 0, 0},
                {-2, 0, 1, 0, 0, 0, 1, 0, 0},
                {0, 0, 3, 0, 0, 2, 0, 0, 0},
                {0, 3, 0, 0, 2, 0, 1, 0, 0},
                {0, 0, 0, 1, 0, 1, 0, 0, 0},
                {0, 0, 0, 0, 0, 0, 4, 0, 0},
                {0, 0, 0, 0, 0, 0, 0, 2, 0}};
        System.out.println();
        System.out.println("graph with negative cycle:");
        shortestPath(graphNegativeCycle, 0);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/graph/BellmanFord.java){:target="_blank" rel="noopener"}

### Solution Notes:
Space Complexity: O(V^2)

Time Complexity: O(V^3)

### Links:
* [Find The Shortest Path (Floyd-Warshall Algorithm)](/blog/find-shortest-path-floyd-warshall)
* [Find The Shortest Path (Dijkstra's Algorithm)](/blog/find-shortest-path-dijkstra)
