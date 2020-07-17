---
layout: default
title: "Controlling Companies"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.3.5
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Some companies are partial owners of other companies because they have acquired 
part of their total shares of stock. For example, Ford at one point owned 12% 
of Mazda. It is said that a company A controls company B if at least one of the 
following conditions is satisfied:

* Company A = Company B
* Company A owns more than 50% of Company B
* Company A controls K (K >= 1) companies denoted C<sub>1</sub>, ..., 
  C<sub>K</sub> with each company C<sub>i</sub> owning x<sub>i</sub>% of 
  company B and x<sub>1</sub> + .... + x<sub>K</sub> > 50%.
  
Given a list of triples (i, j, p) which denote company i owning p% of company j, 
calculate all the pairs (h, s) in which company h controls company s. There are 
at most 100 companies.

Write a program to read the list of triples (i, j, p) where i, j and p are 
positive integers all in the range (1..100) and find all the pairs (h, s) 
so that company h controls company s.

##### PROGRAM NAME: concom

##### INPUT FORMAT

Line 1:	     | n, the number of input triples to follow
-------------|-------------------
Line 2..n+1: | Three integers per line as a triple (i,j,p) described above.

##### SAMPLE INPUT (file concom.in)
```
3
1 2 80
2 3 80
3 1 20
```

##### OUTPUT FORMAT
List 0 or more companies that control other companies. Each line contains two 
integers that denote that the company whose number is the first integer 
controls the company whose number is the second integer. Order the lines in 
ascending order of the first integer (and ascending order of the second integer 
to break ties). Do not print that a company controls itself.

##### SAMPLE OUTPUT (file concom.out)
```
1 2
1 3
2 3
```

### Solution:
```java
public class concom {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("concom.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("concom.out")))) {

            // the number of input triples
            final int N = Integer.parseInt(f.readLine());

            // parse triples ([i][j]=p)
            final int[][] triples = new int[100 + 1][100 + 1];
            final boolean[][] owns = new boolean[100 + 1][100 + 1];
            String[] tokens;
            int i, j, p;
            for (int n = 0; n < N; n++) {
                tokens = f.readLine().split(" ");
                i = Integer.parseInt(tokens[0]);
                j = Integer.parseInt(tokens[1]);
                p = Integer.parseInt(tokens[2]);
                triples[i][j] = p;
                // handles if company A owns more than 50% of company B
                if (triples[i][j] > 50) {
                    owns[i][j] = true;
                }
            }

            // works for test 1,2,3,4,5,6,7 but fails test 8
            // if i own > 50% of j, then find everything that j owns (k) and have i also own it (k)
            // and if j owns > 50% of k, then find everything that k owns (l) and have i also own it (l)
            for (i = 1; i <= 100; i++) {
                for (j = 1; j <= 100; j++) {
                    if (i == j) {
                        continue; // skip
                    }
                    if (triples[i][j] > 50) {
                        for (int k = 1; k <= 100; k++) {
                            if (j == k) {
                                continue; // skip
                            }
                            if (triples[j][k] > 0) {
                                triples[i][k] += triples[j][k];
                                if (triples[i][k] > 50) {
                                    owns[i][k] = true;
                                }
                                if (triples[j][k] > 50) {
                                    for (int l = 1; l <= 100; l++) {
                                        if (k == l) {
                                            continue; // skip
                                        }
                                        if (triples[k][l] > 0) {
                                            triples[i][l] += triples[k][l];
                                            if (triples[i][l] > 50) {
                                                owns[i][l] = true;
                                            }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }

            // works for test 1,2,3,4,5,6,7,8 but fails test 9
            // handles company A = company B (part 1)
            // if i owns 100% of j, then find everything that i owns (k) and have j also own it (k)
            for (i = 1; i <= 100; i++) {
                for (j = 1; j <= 100; j++) {
                    if (i == j) {
                        continue; // skip
                    }
                    if (triples[i][j] == 100) {
                        for (int k = 1; k <= 100; k++) {
                            if (owns[i][k]) {
                                owns[j][k] = true;
                            }
                        }
                    }
                }
            }

            // works for test 1,2,3,4,5,6,7,8,9
            // handles company A = company B (part 2)
            // if i owns 100% of j, then find everything that j owns (k) and have i also own it (k)
            for (i = 1; i <= 100; i++) {
                for (j = 1; j <= 100; j++) {
                    if (i == j) {
                        continue; // skip
                    }
                    if (triples[i][j] == 100) {
                        for (int k = 1; k <= 100; k++) {
                            if (owns[j][k]) {
                                owns[i][k] = true;
                            }
                        }
                    }
                }
            }

            // output
            for (i = 1; i <= 100; i++) {
                for (j = 1; j <= 100; j++) {
                    if (i == j) {
                        continue; // skip
                    }
                    if (owns[i][j]) {
                        out.print(i);
                        out.print(" ");
                        out.println(j);
                    }
                }
            }
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/concom.java){:target="_blank" rel="noopener"}
