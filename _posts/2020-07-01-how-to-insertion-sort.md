---
layout: default
title: "How To Implement Insertion Sort"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, sort, insertion sort]
author: Eddy Yu
published: true
---

This recipe shows how to implement [insertion sort](https://en.wikipedia.org/wiki/Insertion_sort){:target="_blank" rel="noopener"}. 

### Problem:
Given an unsorted array of strings, sort the strings in ascending order.

### Solution:
```java
public class InsertionSort {

    public static void sort(String[] data) {
        for (int i = 1; i < data.length; i++) {
            for (int j = i; j > 0; j--) {
                if (data[j - 1].compareTo(data[j]) > 0) {
                    swap(data, j - 1, j);
                } else {
                    break;
                }
            }
        }
    }

    private static void swap(String[] data, int i, int j) {
        final String temp = data[i];
        data[i] = data[j];
        data[j] = temp;
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

            // sort
            sort(strings);

            // output
            for (String sortedString : strings) {
                System.out.println(sortedString);
            }
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/sort/InsertionSort.java){:target="_blank" rel="noopener"}

### Solution Notes:
Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)
 
### Links:
* [How To Implement Merge Sort](/blog/how-to-merge-sort)

### Sources:
* [Elementary Sorts](https://algs4.cs.princeton.edu/21elementary/)
* [Insertion Sort](https://www.cs.princeton.edu/courses/archive/spring20/cos510/sf/vfa/Sort.html)
* [Implementation Of Insertion Sort](https://algs4.cs.princeton.edu/21elementary/Insertion.java.html)
* [Sorting](https://algs4.cs.princeton.edu/20sorting/)