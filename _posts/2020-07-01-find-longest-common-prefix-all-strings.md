---
layout: default
title: "Find The Longest Common Prefix Among All Strings"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, search, longest common prefix]
author: Eddy Yu
published: true
---

This recipe shows how to find the longest common prefix among all strings.

### Problem:
Given an array of strings, find the longest substring that appears at the
beginning of ALL strings in the array.

### Solution:
This solution leverages the solution to find the 
[longest common prefix between two strings](/blog/find-longest-common-prefix-two-strings){:target="_blank" rel="noopener"}
to implement a solution that works for finding the longest common prefix among 
all strings in an array.
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

    public static String lcp(String[] data) {
        final int N = data.length;
        if (N == 0) {
            return "";
        } else if (N == 1) {
            return data[0];
        } else {
            String prefix = data[0];
            for (String string : data) {
                prefix = lcp(prefix, string);
            }
            return prefix;
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

            // find lcp and output
            System.out.println(lcp(strings));
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/search/LongestCommonPrefix.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Longest Common Prefix Between Two Strings](/blog/find-longest-common-prefix-two-strings)
* [Find The Longest Repeated Substring](/blog/find-longest-repeated-substring)

