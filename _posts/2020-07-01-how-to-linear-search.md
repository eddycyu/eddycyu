---
layout: default
title: "How To Implement Linear Search"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, search, linear search]
author: Eddy Yu
published: true
---

This recipe shows how to implement [linear (or sequential) search](https://en.wikipedia.org/wiki/Linear_search){:target="_blank" rel="noopener"}. 

Linear search is an easy implementation but is generally only practical when 
the search list has only a few elements or when performing a single search.
In other use cases, consider using alternative search algorithms such as 
[binary search](/blog/how-to-binary-search){:target="_blank" rel="noopener"}.

### Problem:
Given an array of strings, search for a given target string in the array.

### Solution:
```java
public class LinearSearch {

    public static int search(String[] data, String target) {
        // search sequentially through the data for the target
        for (int i = 0; i < data.length; i++) {
            if (data[i].compareTo(target) == 0) {
                return i;
            }
        }
        // target not found
        return -1;
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
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/LinearSearch.java){:target="_blank" rel="noopener"}

### Solution Notes:
Time Complexity: O(n)

Space Complexity: O(1)
 
### Links:
* [How To Implement Binary Search](/blog/how-to-binary-search)

### Sources:
* [Searching](https://algs4.cs.princeton.edu/30searching/)
