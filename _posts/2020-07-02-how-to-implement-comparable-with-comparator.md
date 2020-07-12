---
layout: default
title: "How To Implement Comparable With Comparator"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, comparable, comparator]
author: Eddy Yu
published: true
---

This note shows how to implement _Comparable_ objects using _Comparator_ in 
Java for multi-field comparison.

This implementation is more concise but slightly slower than the 
[implementation that does not use _Comparator_](/blog/how-to-implement-comparable).

### Example:
The following example uses a hypothetical _CompareBlock_ object with three 
fields: name, weight, size. We want the blocks to be sorted first by weight
(ascending), and then by size (ascending). We achieve this by defining a
static comparator that is used by the _compareTo()_ method.
```java
public class CompareBlock implements Comparable<CompareBlock> {

    private String name;
    private int weight;
    private int size;

    // multi-field comparator: first lightest block, then smallest block
    private static final Comparator<CompareBlock> COMPARATOR =
            comparingInt((CompareBlock block) -> block.weight)
                    .thenComparingInt(block -> block.size);

    public CompareBlock(String name, int weight, int size) {
        this.name = name;
        this.weight = weight;
        this.size = size;
    }

    // add other constructors, getters, setters, etc...

    @Override
    public int compareTo(CompareBlock o) {
        return COMPARATOR.compare(this, o);
    }

    public static void main(String[] args) {
        final CompareBlock block1 = new CompareBlock("block1", 10, 2);
        final CompareBlock block2 = new CompareBlock("block2", 20, 1);
        final CompareBlock block3 = new CompareBlock("block3", 10, 1);
        final CompareBlock block4 = new CompareBlock("block4", 20, 2);
        final List<CompareBlock> list = new ArrayList<>();
        list.add(block1);
        list.add(block2);
        list.add(block3);
        list.add(block4);
        Collections.sort(list);
        System.out.println("blocks sorted by lightest, then smallest:");
        for (CompareBlock block : list) {
            System.out.println(block.name + " [weight: " + block.weight + "][size: " + block.size + "]");
        }
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/comparison/CompareBlock.java){:target="_blank" rel="noopener"}
