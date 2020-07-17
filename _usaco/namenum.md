---
layout: default
title: "Name That Number"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.3.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Among the large Wisconsin cattle ranchers, it is customary to brand cows with 
serial numbers to please the Accounting Department. The cow hands don't 
appreciate the advantage of this filing system, though, and wish to call the 
members of their herd by a pleasing name rather than saying, "C'mon, #4734, 
get along."

Help the poor cowhands out by writing a program that will translate the brand 
serial number of a cow into possible names uniquely associated with that serial 
number. Since the cow hands all have cellular saddle phones these days, use the 
standard Touch-Tone(R) telephone keypad mapping to get from numbers to letters 
(except for "Q" and "Z"):
```
2: A,B,C     5: J,K,L    8: T,U,V
3: D,E,F     6: M,N,O    9: W,X,Y
4: G,H,I     7: P,R,S
```
Acceptable names for cattle are provided to you in a file named "dict.txt", 
which contains a list of fewer than 5,000 acceptable cattle names (all 
letters capitalized). Take a cow's brand number and report which of all the 
possible words to which that number maps are in the given dictionary which is 
supplied as dict.txt in the grading environment (and is sorted into ascending 
order).

For instance, the brand number 4734 produces all the following names:
```
GPDG GPDH GPDI GPEG GPEH GPEI GPFG GPFH GPFI GRDG GRDH GRDI
GREG GREH GREI GRFG GRFH GRFI GSDG GSDH GSDI GSEG GSEH GSEI
GSFG GSFH GSFI HPDG HPDH HPDI HPEG HPEH HPEI HPFG HPFH HPFI
HRDG HRDH HRDI HREG HREH HREI HRFG HRFH HRFI HSDG HSDH HSDI
HSEG HSEH HSEI HSFG HSFH HSFI IPDG IPDH IPDI IPEG IPEH IPEI
IPFG IPFH IPFI IRDG IRDH IRDI IREG IREH IREI IRFG IRFH IRFI
ISDG ISDH ISDI ISEG ISEH ISEI ISFG ISFH ISFI
```
As it happens, the only one of these 81 names that is in the list of valid 
names is "GREG".

Write a program that is given the brand number of a cow and prints all the 
valid names that can be generated from that brand number or ''NONE'' if there 
are no valid names. Serial numbers can be as many as a dozen digits long.

##### PROGRAM NAME: namenum

##### INPUT FORMAT
A single line with a number from 1 through 12 digits in length.

##### SAMPLE INPUT (file namenum.in)
```
4734
```

##### OUTPUT FORMAT
A list of valid names that can be generated from the input, one per line, in 
ascending alphabetical order.

##### SAMPLE OUTPUT (file namenum.out)
```
GREG
```

### Analysis:
Find all possible names from the number and output the ones that are found in
the dictionary.
    
### Solution:
```java
public class namenum {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("namenum.in"));
             final BufferedReader dict = new BufferedReader(new FileReader("dict.txt"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("namenum.out")))) {

            // get serial number
            final String serialNumber = f.readLine();

            // load dictionary of names
            final Set<String> names = new HashSet<>();
            String line;
            while ((line = dict.readLine()) != null) {
                names.add(line);
            }

            // translate serial number to words
            final List<String> words = new ArrayList<>();
            final char[] digits = serialNumber.toCharArray();
            translate(digits, 0, "", words, names);

            // output answer
            if (words.size() == 0) {
                out.println("NONE");
            } else {
                Collections.sort(words);
                for (String word : words) {
                    out.println(word);
                }
            }
        }
    }

    private static void translate(char[] digits, int index, String word,
                                  List<String> words, Set<String> dict) {
        final String[] letters = getLetters(digits[index]);
        for (String letter : letters) {
            if (index == (digits.length - 1)) {
                final String name = word + letter;
                if (dict.contains(name)) {
                    words.add(name);
                }
            } else {
                translate(digits, index + 1, word + letter, words, dict);
            }
        }
    }

    private static String[] getLetters(char digit) {
        switch (digit) {
            case '2': {
                return new String[]{"A", "B", "C"};
            }
            case '3': {
                return new String[]{"D", "E", "F"};
            }
            case '4': {
                return new String[]{"G", "H", "I"};
            }
            case '5': {
                return new String[]{"J", "K", "L"};
            }
            case '6': {
                return new String[]{"M", "N", "O"};
            }
            case '7': {
                return new String[]{"P", "R", "S"};
            }
            case '8': {
                return new String[]{"T", "U", "V"};
            }
            case '9': {
                return new String[]{"W", "X", "Y"};
            }
            default: {
                return new String[]{};
            }
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/namenum.java){:target="_blank" rel="noopener"}

### Links:
* [Find The Nth Fibonacci Number In The Sequence Using Recursion](/blog/find-nth-fibonacci-number-recursion.html)
