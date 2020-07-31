---
layout: default
title: "How To Implement Merge Sort"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, sort, merge sort]
author: Eddy Yu
published: true
---

This recipe shows how to implement [merge sort](https://en.wikipedia.org/wiki/Merge_sort){:target="_blank" rel="noopener"}. 

### Problem:
Given an unsorted array of strings, sort the strings in ascending order.

### Solution:
```java
public class MergeSort {

    public static void sort(String[] data) {
        final String[] aux = new String[data.length];
        sort(data, aux, 0, data.length);
    }

    private static void sort(String[] data, String[] aux, int lo, int hi) {
        final int N = hi - lo;
        if (N <= 1) {
            return;
        }
        final int mid = lo + N / 2;
        sort(data, aux, lo, mid);
        sort(data, aux, mid, hi);
        merge(data, aux, lo, mid, hi);
    }

    private static void merge(String[] data, String[] aux, int lo, int mid, int hi) {
        // merge data[lo,mid) with data[mid,hi) into aux[0, hi-lo)
        int i = lo;
        int j = mid;
        final int N = hi - lo;
        for (int k = 0; k < N; k++) {
            if (i == mid) {
                aux[k] = data[j++];
            } else if (j == hi) {
                aux[k] = data[i++];
            } else if (data[j].compareTo(data[i]) < 0) {
                aux[k] = data[j++];
            } else {
                aux[k] = data[i++];
            }
        }
        // copy from aux back to data
        if (N >= 0) {
            System.arraycopy(aux, 0, data, lo, N);
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
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/sort/MergeSort.java){:target="_blank" rel="noopener"}

### Solution Notes:
Time Complexity: O(n log n)

Space Complexity: O(n)
 
### Links:
* [How To Implement Linear Sort](/blog/how-to-linear-sort)

