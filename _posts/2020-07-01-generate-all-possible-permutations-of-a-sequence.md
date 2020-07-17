---
layout: default
title: "Generate All Possible Permutations Of A Sequence"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, permutation, recursion, iterator]
author: Eddy Yu
published: true
---

This recipe shows how to generate all possible 
[permutations](https://en.wikipedia.org/wiki/Permutation#Permutations_in_computing){:target="_blank" rel="noopener"}
of a given sequence of integers. We will demonstrate two implementations using 
[Heap's algorithm](https://en.wikipedia.org/wiki/Heap%27s_algorithm){:target="_blank" rel="noopener"}.

### Problem:
Given a sequence of integers, generate all possible permutations.

### Analysis:
Heap's algorithm is widely regarded as the fastest algorithm for generating
permutations in applications. This algorithm can be implemented by using
recursion and iteration.

### Solution 1: Recursion
```java
public class HeapsRecursion {

    private static void permute(int[] sequence, int k) {
        if (k == 1) {
            // output permutation
            System.out.println(Arrays.toString(sequence));
        } else {
            // generate permutations with kth unaltered;
            // initially k == sequence.length
            permute(sequence, k - 1);

            // generate permutation for kth swapped with each k-1 initial
            for (int i = 0; i < k - 1; i++) {
                int temp;
                if (k % 2 == 0) {
                    // if even, the swap the ith and last (k-1) element
                    temp = sequence[i];
                    sequence[i] = sequence[k - 1];
                } else {
                    // else swap the first and last (k-1) element
                    temp = sequence[0];
                    sequence[0] = sequence[k - 1];
                }
                sequence[k - 1] = temp;
                permute(sequence, k - 1);
            }
        }
    }

    public static void main(String[] args) {
        final int[] sequence = {1, 2, 3};

        // kth element in the sequence; initially k == sequence.length
        final int k = sequence.length;

        // generate permutations
        permute(sequence, k);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/permutation/HeapsRecursion.java){:target="_blank" rel="noopener"}

### Solution 1 Output:
```
[1, 2, 3]
[2, 1, 3]
[3, 1, 2]
[1, 3, 2]
[2, 3, 1]
[3, 2, 1]
```
### Solution 2: Iteration
```java
public class HeapsIteration {

    private static void permute(int[] sequence, int n) {

        // c is an encoding of the stack state; c[k] encodes the for-loop
        // counter for when permute(sequence, k+1) is called
        int[] c = new int[n];

        // output permutation
        System.out.println(Arrays.toString(sequence));

        int k = 0;
        while (k < n) {
            if (c[k] < k) {
                int temp;
                if (k % 2 == 0) {
                    // if even, the swap the first and kth element
                    temp = sequence[0];
                    sequence[0] = sequence[k];
                } else {
                    temp = sequence[c[k]];
                    sequence[c[k]] = sequence[k];
                }
                sequence[k] = temp;

                // output permutation
                System.out.println(Arrays.toString(sequence));

                // swap has occurred ending the for-loop; simulate the increment
                // of the for-loop counter
                c[k] += 1;

                // simulate recursive call reaching the base case by bringing
                // the pointer to the base case analogy in the array
                k = 0;
            } else {
                // calling permute(sequence, k+1) has ended as the for-loop
                // terminated; reset the state and simulate popping the stack
                // by incrementing the pointer
                c[k] = 0;
                k++;
            }
        }
    }

    public static void main(String[] args) {
        final int[] sequence = {1, 2, 3};

        // number of element in the sequence
        final int n = sequence.length;

        // generate permutations
        permute(sequence, n);
    }
}
``` 
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/permutation/HeapsIteration.java){:target="_blank" rel="noopener"}

### Solution 2 Output:
```
[1, 2, 3]
[2, 1, 3]
[3, 1, 2]
[1, 3, 2]
[2, 3, 1]
[3, 2, 1]
```

### Notes:
Time Complexity: O(n!)

### Links:
* [How To Reverse A String](/blog/how-to-reverse-a-string)
