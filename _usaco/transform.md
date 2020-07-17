---
layout: default
title: "Transformations"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.3.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
A square pattern of size N x N (1 <= N <= 10) black and white square tiles is 
transformed into another square pattern. Write a program that will recognize 
the minimum transformation that has been applied to the original pattern given 
the following list of possible transformations:

* #1: 90 Degree Rotation: The pattern was rotated clockwise 90 degrees.
* #2: 180 Degree Rotation: The pattern was rotated clockwise 180 degrees.
* #3: 270 Degree Rotation: The pattern was rotated clockwise 270 degrees.
* #4: Reflection: The pattern was reflected horizontally (turned into a mirror image of itself by reflecting around a vertical line in the middle of the image).
* #5: Combination: The pattern was reflected horizontally and then subjected to one of the rotations (#1-#3).
* #6: No Change: The original pattern was not changed.
* #7: Invalid Transformation: The new pattern was not obtained by any of the above methods.

In the case that more than one transform could have been used, choose the one 
with the minimum number above. 

##### PROGRAM NAME: transform

##### INPUT FORMAT

Line 1:          | A single integer, N
-----------------|--------------------
Line 2..N+1:     | N lines of N characters (each either `@' or `-'); this is the square before transformation
-----------------|--------------------
Line N+2..2*N+1: | N lines of N characters (each either `@' or `-'); this is the square after transformation

##### SAMPLE INPUT (file transform.in)
```
3
@-@
---
@@-
@-@
@--
--@
```

##### OUTPUT FORMAT
A single line containing the number from 1 through 7 (described above) that 
categorizes the transformation required to change from the 'before' 
representation to the 'after' representation.

##### SAMPLE OUTPUT (file milk2.out)
```
1
```

### Analysis:
Apply each transformation to the start patten. Then compare each one with the
end pattern to determine which transformation was applied.
    
### Solution:
```java
public class transform {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("transform.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("transform.out")))) {

            final int N = Integer.parseInt(f.readLine());

            // create starting pattern
            final char[][] startPattern = new char[N][N];
            for (int y = 0; y < N; y++) {
                final String input = f.readLine();
                final char[] chars = input.toCharArray();
                for (int x = 0; x < N; x++) {
                    startPattern[x][y] = chars[x];
                }
            }
            final String startString = patternToString(startPattern, N);

            // create ending pattern
            final char[][] endPattern = new char[N][N];
            for (int y = 0; y < N; y++) {
                final String input = f.readLine();
                final char[] chars = input.toCharArray();
                for (int x = 0; x < N; x++) {
                    endPattern[x][y] = chars[x];
                }
            }
            final String endString = patternToString(endPattern, N);

            // generate transformations
            final char[][] transform1 = rotate90(startPattern, N);         // rotate 90 degree
            final char[][] transform2 = rotate90(transform1, N);           // rotate 180 degree
            final char[][] transform3 = rotate90(transform2, N);           // rotate 270 degree
            final char[][] transform4 = rotateHorizontal(startPattern, N); // rotate horizontal
            final char[][] transform5a = rotate90(transform4, N);          // rotate horizontal + 90 degrees
            final char[][] transform5b = rotate90(transform5a, N);         // rotate horizontal + 180 degrees
            final char[][] transform5c = rotate90(transform5b, N);         // rotate horizontal + 270 degrees

            // determine which transformation was used
            int answer = 7; // default: invalid transformation
            final String transform1String = patternToString(transform1, N);
            final String transform2String = patternToString(transform2, N);
            final String transform3String = patternToString(transform3, N);
            final String transform4String = patternToString(transform4, N);
            final String transform5aString = patternToString(transform5a, N);
            final String transform5bString = patternToString(transform5b, N);
            final String transform5cString = patternToString(transform5c, N);
            if (endString.equals(transform1String)) {
                answer = 1;
            } else if (endString.equals(transform2String)) {
                answer = 2;
            } else if (endString.equals(transform3String)) {
                answer = 3;
            } else if (endString.equals(transform4String)) {
                answer = 4;
            } else if (endString.equals(transform5aString)
                    || endString.equals(transform5bString)
                    || endString.equals(transform5cString)) {
                answer = 5;
            } else if (endString.equals(startString)) {
                answer = 6;
            } // else 7 - invalid transformation

            // output answer
            out.println(answer);
        }
    }

    private static String patternToString(char[][] pattern, int length) {
        final StringBuilder buffer = new StringBuilder();
        for (int y = 0; y < length; y++) {
            for (int x = 0; x < length; x++) {
                buffer.append(pattern[x][y]);
            }
        }
        return buffer.toString();
    }

    private static char[][] rotate90(char[][] pattern, int length) {
        final char[][] result = new char[length][length];
        for (int y = 0; y < length; y++) {
            for (int x = 0; x < length; x++) {
                result[length - 1 - y][x] = pattern[x][y];
            }
        }
        return (result);
    }

    private static char[][] rotateHorizontal(char[][] pattern, int length) {
        final char[][] result = new char[length][length];
        for (int y = 0; y < length; y++) {
            for (int x = 0; x < length; x++) {
                result[length - 1 - x][y] = pattern[x][y];
            }
        }
        return (result);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/transform.java){:target="_blank" rel="noopener"}
