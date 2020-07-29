---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 1: Hello, World - Strings And Command-Line Arguments"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [moocs, coursera, programming with a purpose]
order: purpose.1.1
---

Write a program HelloGoodbye.java that takes two names as command-line 
arguments and prints hello and goodbye messages as shown below (with the 
names for the hello message in the same order as the command-line arguments 
and with the names for the goodbye message in reverse order). 

```
~/Desktop/hello> javac HelloGoodbye.java

~/Desktop/hello> java HelloGoodbye Kevin Bob
Hello Kevin and Bob.
Goodbye Bob and Kevin.

~/Desktop/hello> java HelloGoodbye Alejandra Bahati
Hello Alejandra and Bahati.
Goodbye Bahati and Alejandra.
```

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/hello/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class HelloGoodbye {

    public static void main(String[] args) {
        final String name1 = args[0];
        final String name2 = args[1];
        System.out.println("Hello " + name1 + " and " + name2 + ".");
        System.out.println("Goodbye " + name2 + " and " + name1 + ".");
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/HelloGoodbye.java){:target="_blank" rel="noopener"}
