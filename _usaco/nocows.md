---
layout: default
title: "Cow Pedigrees"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco, dynamic programming]
order: 2.3.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Farmer John is considering purchasing a new herd of cows. In this new herd, 
each mother cow gives birth to two children. The relationships among the 
cows can easily be represented by one or more binary trees with a total of 
N (3 <= N < 200) nodes. The trees have these properties:

* The degree of each node is 0 or 2. The degree is the count of the node's 
  immediate children.
* The height of the tree is equal to K (1 < K < 100). The height is the 
  number of nodes on the longest path from the root to any leaf; a leaf 
  is a node with no children.

How many different possible pedigree structures are there? A pedigree is 
different if its tree structure differs from that of another pedigree. 
Output the remainder when the total number of different possible pedigrees 
is divided by 9901.

##### PROGRAM NAME: nowcows

##### INPUT FORMAT
Line 1: Two space-separated integers, N and K.

##### SAMPLE INPUT (file nowcows.in)
```
5 3
```

##### OUTPUT FORMAT
Line 1: One single integer number representing the number of possible pedigrees 
MODULO 9901.

##### SAMPLE OUTPUT (file nocows.out)
```
2
```

##### OUTPUT DETAILS
Two possible pedigrees have 5 nodes and height equal to 3:
```
          @                   @      
          / \                 / \
         @   @      and      @   @
        / \                     / \
       @   @                   @   @
```

### Solution:
```java
public class nocows {

    public static void main(String[] args) throws IOException {

        try (final BufferedReader f = new BufferedReader(new FileReader("nocows.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("nocows.out")))) {

            final String line = f.readLine();
            final String[] tokens = line.split(" ");
            final int N = Integer.parseInt(tokens[0]);
            final int K = Integer.parseInt(tokens[1]);

            // trees[n][k] = the number of trees with node count (n) and height (k)
            int[][] trees = new int[200 + 1][100 + 1];
            for (int n = 1; n <= N; n++) {
                for (int k = 1; k <= K; k++) {
                    if (n == 1) {
                        trees[n][k] = 1;
                    } else {
                        for (int m = 1; m <= n - 2; m++) {
                            /*
                             * m = number of nodes in left subtree
                             * n-m-1 = number of nodes in right subtree
                             *
                             * sum up while iterating from 1 node on left subtree (and rest in right subtree)
                             * until 1 node in right subtree (and rest in left subtree)
                             */
                            trees[n][k] += trees[m][k - 1] * trees[n - m - 1][k - 1];
                            trees[n][k] %= 9901;
                        }
                    }
                }
            }

            // output result
            int result = (trees[N][K] - trees[N][K - 1] + 9901) % 9901;
            out.println(result);
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/nocows.java){:target="_blank" rel="noopener"}

### Links:
* [Subset Sums](/usaco/subset)
* [Broken Necklace](/usaco/beads)
* [Find The Nth Fibonacci Number In The Sequence Using Dynamic Programming](/blog/find-nth-fibonacci-number-dynamic-programming.html)
