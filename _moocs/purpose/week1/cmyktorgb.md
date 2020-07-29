---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 1: Hello, World - Type Conversion"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.1.4
---

Several different formats are used to represent color. For example, the primary
format for LCD displays, digital cameras, and web pages — known as the RGB 
format — specifies the level of red (R), green (G), and blue (B) on an integer 
scale from 0 to 255. The primary format for publishing books and magazines 
— known as the CMYK format — specifies the level of cyan (C), magenta (M), 
yellow (Y), and black (K) on a real scale from 0.0 to 1.0.

Write a program CMYKtoRGB.java that converts from CMYK format to RGB format 
using these mathematical formulas: 

```
white = 1 - black
red   = 255 x white x (1 - cyan)
green = 255 x white x (1 - magenta)
blue  = 255 x white x (1 - yellow)
```

Your program must take four double command-line arguments cyan, magenta, 
yellow, and black; compute the corresponding RGB values, each rounded to the 
nearest integer; and print the RGB values, as in the following sample 
executions: 

```
~/Desktop/hello> javac CMYKtoRGB.java

~/Desktop/hello> java CMYKtoRGB 0.0 1.0 0.0 0.0    // magenta
red   = 255
green = 0
blue  = 255

~/Desktop/hello> java CMYKtoRGB 0.0 0.4392156862745098 1.0 0.0    // Princeton orange
red   = 255
green = 143
blue  = 0
```

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/hello/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class CMYKtoRGB {

    public static void main(String[] args) {
        final double c = Double.parseDouble(args[0]);
        final double m = Double.parseDouble(args[1]);
        final double y = Double.parseDouble(args[2]);
        final double k = Double.parseDouble(args[3]);

        final double w = 1.0 - k;
        final double r = 255.0 * w * (1.0 - c);
        final double g = 255.0 * w * (1.0 - m);
        final double b = 255.0 * w * (1.0 - y);

        System.out.println("red   = " + Math.round(r));
        System.out.println("green = " + Math.round(g));
        System.out.println("blue  = " + Math.round(b));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/CMYKtoRGB.java){:target="_blank" rel="noopener"}
