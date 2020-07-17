---
layout: default
title: "Milking Cows"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.3.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Three farmers rise at 5 am each morning and head for the barn to milk three 
cows. The first farmer begins milking his cow at time 300 (measured in 
seconds after 5 am) and ends at time 1000. The second farmer begins at time 
700 and ends at time 1200. The third farmer begins at time 1500 and ends at 
time 2100. The longest continuous time during which at least one farmer was 
milking a cow was 900 seconds (from 300 to 1200). The longest time no 
milking was done, between the beginning and the ending of all milking, was 
300 seconds (1500 minus 1200).

Your job is to write a program that will examine a list of beginning and 
ending times for N (1 <= N <= 5000) farmers milking N cows and compute 
(in seconds):

* The longest time interval at least one cow was milked.
* The longest time interval (after milking starts) during which no cows were being milked.
 
##### PROGRAM NAME: milk2

##### INPUT FORMAT

Line 1:       | The single integer, N
--------------|----------------------
Lines 2..N+1: | Two non-negative integers less than 1,000,000, respectively the starting and ending time in seconds after 0500

##### SAMPLE INPUT (file milk2.in)
```
3
300 1000
700 1200
1500 2100
```

##### OUTPUT FORMAT
A single line with two integers that represent the longest continuous time of 
milking and the longest idle time.

##### SAMPLE OUTPUT (file milk2.out)
```
900 300
```

### Analysis:
The basic approach is to combine overlapping milking times into the same segment.
Then the longest continuous milking time would be the longest segment, and the
longest idle time would be the longest distance between two adjacent segments.
    
### Solution:
```java
public class milk2 {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("milk2.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("milk2.out")))) {

            // number of farmers/cow
            final int N = Integer.parseInt(f.readLine());

            // beginning and ending times
            final ArrayList<Node> cowList = new ArrayList<>();
            for (int i = 0; i < N; i++) {
                final String input = f.readLine();
                final String[] tokens = input.split(" ");
                cowList.add(new Node(Integer.parseInt(tokens[0]), Integer.parseInt(tokens[1])));
            }

            // sort in ascending order by beginning time
            Collections.sort(cowList);

            // find segments (combine overlapping beginning/ending times into same segment)
            final ArrayList<Node> segmentList = new ArrayList<>();
            if (cowList.size() == 1) {
                segmentList.add(cowList.get(0));
            } else {
                int earliest = cowList.get(0).begin;
                int latest = cowList.get(0).end;
                for (int i = 1; i < cowList.size(); i++) {
                    final Node current = cowList.get(i);
                    if (current.begin > latest) {
                        segmentList.add(new Node(earliest, latest));
                        earliest = current.begin;
                        latest = current.end;
                    }
                    if (current.end > latest) {
                        latest = current.end;
                    }
                }
                segmentList.add(new Node(earliest, latest));
            }

            // find longest milking time (e.g. longest segment)
            int longestMilkingTime = 0;
            for (Node current : segmentList) {
                final int length = current.end - current.begin;
                if (length > longestMilkingTime) {
                    longestMilkingTime = length;
                }
            }

            // find longest non-milking time
            // (e.g. longest distance between adjacent segments)
            int longestNonMilkingTime = 0;
            if (segmentList.size() > 0) {
                Node previous = segmentList.get(0);
                for (int i = 1; i < segmentList.size(); i++) {
                    final Node current = segmentList.get(i);
                    final int length = current.begin - previous.end;
                    if (length > longestNonMilkingTime) {
                        longestNonMilkingTime = length;
                    }
                    previous = current;
                }
            }

            // output the answer
            final String answer = longestMilkingTime + " " + longestNonMilkingTime;
            out.println(answer);
        }
    }

    public static class Node implements Comparable<Node> {
        int begin;
        int end;

        Node(int begin, int end) {
            super();
            this.begin = begin;
            this.end = end;
        }

        @Override
        public String toString() {
            return ("[" + this.begin + "," + this.end + "]");
        }

        @Override
        public int compareTo(Node o) {
            if (begin < o.begin) {
                return -1;
            } else if (begin > o.begin) {
                return 1;
            }
            return 0;
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/milk2.java){:target="_blank" rel="noopener"}
