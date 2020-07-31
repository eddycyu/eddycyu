---
layout: default
title: "Find The Longest Repeated Substring"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, search, longest repeated substring]
author: Eddy Yu
published: true
---

This recipe shows how to find the longest repeated substring in a string.

### Problem:
Given an input string, find the longest substring that occurs at least twice 
in the string.

### Solution:
This solution uses a [suffix array](https://en.wikipedia.org/wiki/Suffix_array){:target="_blank" rel="noopener"}
to solve the problem.
```java
public class LongestRepeatedSubstring {

    public static String lrs(String input) {
        // form suffix array
        final int N = input.length();
        final String[] suffixes = new String[N];
        for (int i = 0; i < N; i++) {
            suffixes[i] = input.substring(i, N);
        }

        // sort the suffix array in ascending order
        MergeSort.sort(suffixes);

        // find LCP (longest common prefix) among adjacent entries
        String lrs = "";
        for (int i = 0; i < N - 1; i++) {
            final String lcp = LongestCommonPrefix.lcp(suffixes[i], suffixes[i + 1]);
            if (lcp.length() > lrs.length()) {
                lrs = lcp;
            }
        }
        return lrs;
    }

    public static void main(String[] args) {
        final String input = args[0];
        System.out.println(lrs(input));
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/LongestRepeatedSubstring.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Longest Common Prefix Between Two Strings](/blog/find-longest-common-prefix-two-strings)
* [Find The Longest Common Prefix Among All Strings](/blog/find-longest-common-prefix-all-strings)
* [How To Implement Merge Sort](/blog/how-to-merge-sort)

