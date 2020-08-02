---
layout: default
title: "How To Implement Binary Search"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, recursion, iteration, search, binary search]
author: Eddy Yu
published: true
---

This recipe shows how to implement [binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm){:target="_blank" rel="noopener"}. 
We will explore two different approaches to implementing the solution.

Note: Binary search requires that the data to be searched is already sorted in 
ascending order.

### Problem:
Given a sorted array of strings, search for a given target string in the array.

### Solution 1 (Iteration):
This solution demonstrates an implementation using iteration.
```java
public class BinarySearchIteration {

    public static int search(String[] data, String target) {
        int lo = 0;
        int hi = data.length - 1;
        while (lo <= hi) {
            final int mid = lo + (hi - lo) / 2;
            final int result = data[mid].compareTo(target);
            if (result > 0) {
                // mid is larger than target; check lower half
                hi = mid - 1;
            } else if (result < 0) {
                // mid is smaller than target; check upper half
                lo = mid + 1;
            } else {
                // found target at mid point
                return mid;
            }
        }
        // target not found
        return -1;
    }

    public static void main(String[] args) throws IOException {
        try (final BufferedReader bf = new BufferedReader(new InputStreamReader(System.in))) {
            // read data
            String line;
            final List<String> data = new ArrayList<>();
            while (true) {
                line = bf.readLine();
                if ((line == null) || line.isEmpty()) {
                    break;
                }
                data.add(line);
            }

            // terminate if no data was read
            if (data.isEmpty()) {
                return;
            }

            // convert list to array
            final String[] strings = data.toArray(new String[0]);

            // sort the data in the array
            Arrays.sort(strings);

            // pick random target from data
            final String target = strings[(int) (Math.random() * strings.length)];

            // search for value
            final int result = search(strings, target);
            if (result != -1) {
                System.out.println("Found target [" + target + "] at index [" + result + "].");
            } else {
                System.out.println("Unable to find target [" + target + "].");
            }
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/BinarySearchIteration.java){:target="_blank" rel="noopener"}

### Solution 1 Notes:
Time Complexity: O(log n)

Space Complexity: O(1)

### Solution 2 (Recursion):
This solution demonstrates an implementation using recursion.
```java
public class BinarySearchRecursion {

    public static int search(String[] data, String target) {
        return search(data, target, 0, data.length);
    }

    private static int search(String[] data, String target, int lo, int hi) {
        if (hi <= lo) {
            // target not found
            return -1;
        }
        final int mid = lo + (hi - lo) / 2;
        final int result = data[mid].compareTo(target);
        if (result > 0) {
            // mid is larger than target; check lower half
            return search(data, target, lo, mid);
        } else if (result < 0) {
            // mid is smaller than target; check upper half
            return search(data, target, mid + 1, hi);
        } else {
            // found target at mid point
            return mid;
        }
    }

    public static void main(String[] args) throws IOException {
        try (final BufferedReader bf = new BufferedReader(new InputStreamReader(System.in))) {
            // read data from standard input
            String line;
            final List<String> data = new ArrayList<>();
            while (true) {
                line = bf.readLine();
                if ((line == null) || line.isEmpty()) {
                    break;
                }
                data.add(line);
            }

            // terminate if no data was read
            if (data.isEmpty()) {
                return;
            }

            // convert list to array
            final String[] strings = data.toArray(new String[0]);

            // sort the data in the array
            Arrays.sort(strings);

            // pick random target from data
            final String target = strings[(int) (Math.random() * strings.length)];

            // search for target
            final int result = search(strings, target);
            if (result != -1) {
                System.out.println("Found target [" + target + "] at index [" + result + "].");
            } else {
                System.out.println("Unable to find target [" + target + "].");
            }
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/BinarySearchRecursion.java){:target="_blank" rel="noopener"}

### Solution 2 Notes: 
Time Complexity: O(log n)

Space Complexity: O(log n)
 
### Links:
* [How To Implement Linear Search](/blog/how-to-linear-search)

### Sources:
* [Binary Search Trees](https://algs4.cs.princeton.edu/32bst/)
* [Implementation Of Binary Search](https://algs4.cs.princeton.edu/11model/BinarySearch.java.html)
* [Searching](https://algs4.cs.princeton.edu/30searching/)