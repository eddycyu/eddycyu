---
layout: default
title: "Healthy Holsteins"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.1.4
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Farmer John prides himself on having the healthiest dairy cows in the world. 
He knows the vitamin content for one scoop of each feed type and the minimum 
daily vitamin requirement for his cows. Help Farmer John feed the cows so 
they stay healthy while minimizing the number of scoops that a cow is fed.

Given the daily requirements of each kind of vitamin that a cow needs, identify
the smallest combination of scoops of feed a cow can be fed in order to meet at 
least the minimum vitamin requirements.

Vitamins are measured in integer units. Cows can be fed at most one scoop of any 
feed type. It is guaranteed that a solution exists for all contest input data.

##### PROGRAM NAME: holstein

##### INPUT FORMAT

Line 1:       | integer V (1 <= V <= 25), the number of types of vitamins
--------------|---------------------
Line 2:       | V integers (1 <= each one <= 1000), the minimum requirement for each of the V vitamins that a cow requires each day
Line 3:       | integer G (1 <= G <= 15), the number of types of feeds available
Lines 4..G+3: | V integers (0 <= each one <= 1000), the amount of each vitamin that one scoop of this feed contains. The first line of these G lines describes feed #1; the second line describes feed #2; and so on.

##### SAMPLE INPUT (file holstein.in)
```
4
100 200 300 400
3
50   50  50  50
200 300 200 300
900 150 389 399
```

##### OUTPUT FORMAT
The output is a single line of output that contains:

* the minimum number of scoops a cow must eat, followed by:
* a SORTED list (from smallest to largest) of the feed types the cow is given

If more than one set of feedtypes yield a minimum of scoops, choose the set 
with the smallest feedtype numbers.

##### SAMPLE OUTPUT (file holstein.out)
```
2 1 3
```
    
### Solution:
```java
public class holstein {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("holstein.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("holstein.out")))) {

            // number of types of vitamins
            final int V = Integer.parseInt(f.readLine());

            // minimum requirement for each vitamin
            final int[] minimum = new int[V];
            final String[] tokens = f.readLine().split(" ");
            for (int i = 0; i < V; i++) {
                minimum[i] = Integer.parseInt(tokens[i]);
            }

            // number of types of feeds
            final int G = Integer.parseInt(f.readLine());

            // amount of each vitamin found in one scoop of each feed
            final int[][] feeds = new int[G][V];
            for (int g = 0; g < G; g++) {
                final Scanner scanner = new Scanner(f.readLine());
                for (int v = 0; v < V; v++) {
                    feeds[g][v] = scanner.nextInt();
                }
                scanner.close();
            }

            // find solution
            final Set<Integer> seen = new HashSet<>();
            final List<Result> results = new ArrayList<>();
            for (int i = 0; i < G; i++) {
                final int[] scoops = new int[G];
                final int[] amounts = new int[V];
                addScoop(i, scoops, amounts, feeds);
                findScoops(scoops, amounts, minimum, feeds, G, seen, results);
            }

            // output result
            final Result bestResult = results.get(results.size() - 1);
            out.print(bestResult.scoopCount);
            for (int i = 0; i < bestResult.scoops.length; i++) {
                if (bestResult.scoops[i] == 1) {
                    out.print(" " + (i + 1));
                }
            }
            out.println();
        }
    }

    private static boolean findScoops(int[] scoops, int[] amounts, int[] minimum,
                                      int[][] feeds, int feedCount,
                                      Set<Integer> seen, List<Result> results) {
        if (isMeetsMinimum(amounts, minimum)) {
            final Result result = new Result(scoops, amounts);
            results.add(result);
            return true; // found a solution
        }

        // optimization: try adding another scoop, but only if we don't
        // already have an answer with fewer scoops
        int scoopCount = getScoopCount(scoops);
        int minimumScoopCount = getMinimumScoopCount(results);
        if ((minimumScoopCount > 0) && (scoopCount == minimumScoopCount)) {
            return true; // already found a better solution previously
        }

        for (int i = 0; i < feedCount; i++) {
            final int[] newScoops = Arrays.copyOf(scoops, scoops.length);
            final int[] newAmounts = Arrays.copyOf(amounts, amounts.length);
            if (newScoops[i] != 1) {
                addScoop(i, newScoops, newAmounts, feeds);
                // optimization: make sure we didn't already compute this combination before
                final int newScoopKey = scoopsToKey(newScoops);
                if (!seen.contains(newScoopKey)) {
                    seen.add(newScoopKey);
                    // check if the new scoop combo is a solution
                    if (findScoops(newScoops, newAmounts, minimum, feeds, feedCount, seen, results)) {
                        // found solution; no need to try next combo in this loop
                        break;
                    }
                }
            }
        }
        return false; // keep looking for a solution
    }

    private static void addScoop(int index, int[] scoops, int[] amounts, int[][] feeds) {
        scoops[index] = 1;
        for (int i = 0; i < amounts.length; i++) {
            amounts[i] += feeds[index][i];
        }
    }

    private static boolean isMeetsMinimum(int[] amounts, int[] minimum) {
        for (int i = 0; i < amounts.length; i++) {
            if (amounts[i] < minimum[i]) {
                return false;
            }
        }
        return true;
    }

    private static int getScoopCount(int[] scoops) {
        int scoopCount = 0;
        for (int scoop : scoops) {
            if (scoop == 1) {
                scoopCount++;
            }
        }
        return scoopCount;
    }

    private static int getMinimumScoopCount(List<Result> results) {
        if (results.isEmpty()) {
            return 0;
        }
        Result bestResult = results.get(results.size() - 1);
        return bestResult.scoopCount;
    }

    private static int scoopsToKey(int[] scoops) {
        int key = 0;
        for (int i = 0; i < scoops.length; i++) {
            if (scoops[i] == 1) {
                key = key | (1 << i);
            }
        }
        return key;
    }

    static class Result {
        int[] scoops;
        int[] amount;
        int scoopCount;

        private Result() {
            super();
        }

        Result(int[] scoops, int[] amount) {
            this();
            this.scoops = scoops;
            this.amount = amount;
            for (int scoop : scoops) {
                if (scoop == 1) {
                    scoopCount++;
                }
            }
        }

        @Override
        public boolean equals(Object obj) {
            if (obj == null) {
                return false;
            }
            if (!Result.class.isAssignableFrom(obj.getClass())) {
                return false;
            }
            final Result other = (Result) obj;
            return Arrays.equals(this.scoops, other.scoops);
        }

        @Override
        public int hashCode() {
            int hash = 3;
            for (int scoop : scoops) {
                hash = 53 * hash + scoop;
            }
            return hash;
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/holstein.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion)
