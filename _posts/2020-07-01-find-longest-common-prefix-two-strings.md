---
layout: default
title: "Find The Longest Common Prefix Between Two Strings"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, search, longest common prefix]
author: Eddy Yu
published: true
---

This recipe shows how to find the longest common prefix between two strings.

### Problem:
Given two strings _a_ and _b_, find the longest substring that appears at the
beginning of both strings.

### Solution:
```java
public class LongestCommonPrefix {

    public static String lcp(String a, String b) {
        final int N = Math.min(a.length(), b.length());
        for (int i = 0; i < N; i++) {
            if (a.charAt(i) != b.charAt(i)) {
                return a.substring(0, i);
            }
        }
        return a.substring(0, N);
    }

    public static void main(String[] args) {
        final String a = args[0];
        final String b = args[1];
        System.out.println(lcp(a, b));
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/LongestCommonPrefix.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Longest Common Prefix Among All Strings](/blog/find-longest-common-prefix-all-strings)
* [Find The Longest Repeated Substring](/blog/find-longest-repeated-substring)

