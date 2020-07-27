---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 8: Abstract Data Types - Huntington's Disease Detector"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.8.1
---

Huntington’s disease is an inherited and fatal neurological disorder. Although 
there is currently no cure, in 1993 scientists discovered a very accurate 
genetic test. The gene that causes Huntington's disease is located on 
chromosome 4 and has a variable number of (consecutive) repeats of the CAG 
trinucleotide. The normal range of CAG repeats is between 10 and 35. 
Individuals with Huntington's disease have between 36 and 180 repeats. 
Doctors can use a PCR-based DNA test; count the maximum number of repeats; and 
use the following table to generate a diagnosis:

repeats | diagnosis
--------|----------
0-9     | not human
10-35   | normal
36-39   | high risk
40-180  | Huntington's
181-    | not human

Write a program Huntingtons.java to analyze a DNA string for Huntington's 
disease and produce a diagnosis. To do so, implement the following public 
API:

```java
public class Huntingtons {

    // Returns the maximum number of consecutive repeats of CAG in the DNA string.
    public static int maxRepeats(String dna)

    // Returns a copy of s, with all whitespace (spaces, tabs, and newlines) removed.
    public static String removeWhitespace(String s)

    // Returns one of these diagnoses corresponding to the maximum number of repeats:
    // "not human", "normal", "high risk", or "Huntington's".
    public static String diagnose(int maxRepeats)

    // Sample client (see below).
    public static void main(String[] args)
}
```

Here is some more information about the required behavior:
* _Maximum CAG repeats._ We recommend that you use the _substring()_, 
  _equals()_, and _length()_ methods from the _String_ library.
* _Remove whitespace._ We recommend that you use the _replace()_ method from 
  the _String_ library. Use \n to specify a newline character and \t to specify a 
  tab character.
* _Performance requirement._ The _maxRepeats()_ and _removeWhitespace()_ 
  methods must take time linear in the length of the string.
* _Sample client._ The _main()_ method should
  * Take the name of a file as a command-line argument.
  * Read the genetic sequence from the file using the _In_ class.
  * Remove any whitespace (spaces, tabs, and newlines).
  * Count the number of CAG repeats.
  * Print a medical diagnosis in the format below.
* _Input format._ The file format is a sequence of nucleotides (A, C, G, and T), 
  with arbitrary amounts of whitespace separating the nucleotides. The data 
  files 
  [repeats4.txt](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/repeats4.txt){:target="_blank" rel="noopener"},
  [repeats64.txt](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/repeats64.txt){:target="_blank" rel="noopener"},
  [chromosome4-hd.txt](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/chromosome4-hd.txt){:target="_blank" rel="noopener"},
  [chromosome4-healthy.txt](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/chromosome4-healty.txt){:target="_blank" rel="noopener"} 
  are in the specified format.

Here are a few sample executions:
```
~/Desktop/oop1> cat repeats4.txt
TTTTTTTTTT TTTTTTTTTT TTTTTTTTCAGCAGCAGCAG TTTCAGCAGT TTTTTTTTTT
TTTTTTTTTT TTTTTTTTTT TTTTTTTTTTTTTCAGTTTT TTTTTTTTTT T

~/Desktop/oop1> java Huntingtons repeats4.txt
max repeats = 4
not human

~/Desktop/oop1> cat repeats64.txt
TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT TTCAGCAGCA
GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA
GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA
GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA GCAGCAGCAG CAGCAGCAGC AGCAGCAGCA
GCAGTTTTTT TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT TTTTTTTTTT

~/Desktop/oop1> java Huntingtons repeats64.txt
max repeats = 64
Huntington's

~/Desktop/oop1> java Huntingtons chromosome4-hd.txt
max repeats = 79
Huntington's

~/Desktop/oop1> java Huntingtons chromosome4-healthy.txt
max repeats = 19
normal
```

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/oop1/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class Huntingtons {

    // Returns the maximum number of consecutive repeats of CAG in the DNA string.
    public static int maxRepeats(String dna) {
        int previousJ = -1;
        int count = 0;
        int maxCount = 0;
        int i = 0;
        while (i < dna.length()) {
            int j = dna.indexOf("CAG", i);
            if (j == -1) {
                // no more occurrence of CAG is found
                break;
            } else if ((previousJ == -1) || (previousJ == (j - 3))) {
                // first find or found consecutive
                count++;
            } else {
                // non-consecutive; restart count
                count = 1;
            }
            maxCount = Math.max(maxCount, count);
            previousJ = j;
            i = j + 3;
        }
        return maxCount;
    }

    // Returns a copy of s, with all whitespace (spaces, tabs, and newlines) removed.
    public static String removeWhitespace(String s) {
        return s.replace("\t", "").replace("\n", "").replace(" ", "");
    }

    // Returns one of these diagnoses corresponding to the maximum number of repeats:
    // "not human", "normal", "high risk", or "Huntington's".
    public static String diagnose(int maxRepeats) {
        if ((maxRepeats >= 0) && (maxRepeats <= 9)) {
            return "not human";
        } else if ((maxRepeats >= 10) && (maxRepeats <= 35)) {
            return "normal";
        } else if ((maxRepeats >= 36) && (maxRepeats <= 39)) {
            return "high risk";
        } else if ((maxRepeats >= 40) && (maxRepeats <= 180)) {
            return "Huntington's";
        } else {
            return "not human";
        }
    }

    // Sample client (see below).
    public static void main(String[] args) {
        final String dna = removeWhitespace(new In(args[0]).readAll());
        final int count = maxRepeats(dna);
        StdOut.println("max repeats = " + count);
        StdOut.println(diagnose(count));
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/Huntingtons.java){:target="_blank" rel="noopener"}
