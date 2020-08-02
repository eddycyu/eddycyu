---
layout: default
title: "How To Implement A Symbol Table Using A Binary Search Tree (BST)"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, sort, symbol table, binary search tree]
author: Eddy Yu
published: true
---

This recipe shows how to implement a [symbol table](https://en.wikipedia.org/wiki/Symbol_table){:target="_blank" rel="noopener"}
using a [binary search tree (BST)](https://en.wikipedia.org/wiki/Binary_search_tree){:target="_blank" rel="noopener"}.
We will use the symbol table to keep track of the frequency that strings repeat 
in an unsorted array.

A benefit of using a binary search tree is that is provides an ordering for the 
keys. A drawback is that the performance suffers when the keys arrive in sorted
order.

The best case scenario for using a binary search tree to implement a 
symbol table is when the keys arrive in random order. This results in a tree
that is roughly balanced whereby reducing the total number of compares needed to 
find any given key. The worst case scenario is when the keys arrive in sorted 
order, which would essentially result in a linked list and significantly impact 
the search performance for finding any given key.

The time complexity of using a binary search tree is O(n log n). For better 
performance and to address the worst case scenario when keys arrive in sorted
order, a symbol table can be implemented using a
[left-leaning red-black tree](https://en.wikipedia.org/wiki/Left-leaning_red%E2%80%93black_tree){:target="_blank" rel="noopener"},
a self-balancing binary search tree, which would provide a time complexity of
O(log n).

### Problem:
Given an unsorted array of strings, output each string in sorted (ascending)
order along with the frequency that the string repeats in the array.

### Solution:
```java
public class SymbolTableBST<Key extends Comparable<Key>, Value> {

    private Node root = null;

    private class Node {
        private final Key key;
        private Value value;
        private Node left;
        private Node right;

        public Node(Key key, Value value) {
            super();
            this.key = key;
            this.value = value;
        }
    }

    public boolean isEmpty() {
        return root == null;
    }

    public void put(Key key, Value value) {
        root = put(root, key, value);
    }

    private Node put(Node node, Key key, Value value) {
        if (node == null) {
            return new Node(key, value);
        }
        final int result = key.compareTo(node.key);
        if (result < 0) {
            // key value is less than node's key value; check left node
            node.left = put(node.left, key, value);
        } else if (result > 0) {
            // key value is greater than node's key value; check right node
            node.right = put(node.right, key, value);
        } else {
            node.value = value;
        }
        return node;
    }

    public Value get(Key key) {
        return get(root, key);
    }

    private Value get(Node node, Key key) {
        while (node != null) {
            final int result = key.compareTo(node.key);
            if (result < 0) {
                // key value is less than node's key value; check left node
                node = node.left;
            } else if (result > 0) {
                // key value is greater than node's key value; check right node
                node = node.right;
            } else {
                return node.value;
            }
        }
        return null;
    }

    public boolean contains(Key key) {
        return get(key) != null;
    }

    public Iterable<Key> keys() {
        final LinkedList<Key> queue = new LinkedList<>();
        inorder(root, queue);
        return queue;
    }

    public void inorder(Node node, List<Key> queue) {
        if (node == null) {
            return;
        }
        inorder(node.left, queue);
        queue.add(node.key);
        inorder(node.right, queue);
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

            // add key-value pairs to symbol table and keep track of frequency
            final SymbolTableBST<String, Integer> freq = new SymbolTableBST<>();
            for (String key : strings) {
                if (freq.contains(key)) {
                    freq.put(key, freq.get(key) + 1);
                } else {
                    freq.put(key, 1);
                }
            }

            // output frequency
            for (String key : freq.keys()) {
                System.out.printf("%s : %d\n", key, freq.get(key));
            }
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/collection/SymbolTableBST.java){:target="_blank" rel="noopener"}

### Solution Notes:
Time Complexity: O(n log n)

### Links:
* [How To Implement Linear Search](/blog/how-to-linear-search)
* [How To Implement Binary Search](/blog/how-to-binary-search)
* [How To Implement Insertion Sort](/blog/how-to-insertion-sort)
* [How To Implement Merge Sort](/blog/how-to-merge-sort)

### Sources:
* [Symbol Tables](https://www.cs.princeton.edu/courses/archive/fall14/cos126/lectures/15-SymbolTables.pdf)
* [Left-Leaning Red-Black Trees](https://www.cs.princeton.edu/~rs/talks/LLRB/RedBlack.pdf)
* [Implementation Of A Symbol Table Using A Left-Leaning Red-Black BST](https://algs4.cs.princeton.edu/33balanced/RedBlackBST.java.html)
* [Balanced Search Trees](https://algs4.cs.princeton.edu/33balanced/)