---
layout: default
title: "How To Implement Comparable"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, comparable]
author: Eddy Yu
published: true
---

This note shows how to implement _Comparable_ objects in Java for multi-field
comparison.

Unlike the _equals()_ method in objects which only allows for equality 
comparison, the _compareTo()_ method in objects that implement the _Comparable_ 
interface allows for order comparison.

### Example:
The following example uses a hypothetical _ComparableBlock_ object with three 
fields: name, weight, size. We want the blocks to be sorted first by weight
(ascending), and then by size (ascending). 
```java
public class ComparableBlock implements Comparable<ComparableBlock> {

    private String name;
    private int weight;
    private int size;

    public ComparableBlock(String name, int weight, int size) {
        this.name = name;
        this.weight = weight;
        this.size = size;
    }

    // add other constructors, getters, setters, etc...

    @Override
    public int compareTo(ComparableBlock o) {
        // multi-field comparison: first lightest block, then smallest block
        int result = Integer.compare(weight, o.weight);
        if (result == 0) {
            result = Integer.compare(size, o.size);
        }
        return result;
    }

    public static void main(String[] args) {
        final ComparableBlock block1 = new ComparableBlock("block1", 10, 2);
        final ComparableBlock block2 = new ComparableBlock("block2", 20, 1);
        final ComparableBlock block3 = new ComparableBlock("block3", 10, 1);
        final ComparableBlock block4 = new ComparableBlock("block4", 20, 2);
        final List<ComparableBlock> list = new ArrayList<>();
        list.add(block1);
        list.add(block2);
        list.add(block3);
        list.add(block4);
        Collections.sort(list);
        System.out.println("blocks sorted by lightest, then smallest:");
        for (ComparableBlock block : list) {
            System.out.println(block.name + " [weight: " + block.weight + "][size: " + block.size + "]");
        }
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/comparison/ComparableBlock.java){:target="_blank" rel="noopener"}
