---
layout: default
title: "Zero Sum"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 2.3.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Consider the sequence of digits from 1 through N (where N=9) in increasing 
order: 1 2 3 ... N.

Now insert either a '+' for addition or a '-' for subtraction or a ' ' \[blank] 
to run the digits together between each pair of digits (not in front of the 
first digit). Calculate the result of the expression and see if you get zero.

Write a program that will find all sequences of length N that produce a zero 
sum.

##### PROGRAM NAME: zerosum

##### INPUT FORMAT
A single line with the integer N (3 <= N <= 9).

##### SAMPLE INPUT (file zerosum.in)
```
7
```

##### OUTPUT FORMAT
In ASCII order, show each sequence that can create 0 sum with a '+', '-', or 
' ' between each pair of numbers.

##### SAMPLE OUTPUT (file zerosum.out)
```
1+2-3+4-5-6+7
1+2-3-4+5+6-7
1-2 3+4+5+6+7
1-2 3-4 5+6 7
1-2+3+4-5+6-7
1-2-3-4-5+6+7
```

### Solution:
```java
public class zerosum {

    public static void main(String[] args) throws IOException {

        try (final BufferedReader f = new BufferedReader(new FileReader("zerosum.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("zerosum.out")))) {

            // sequence length
            final int N = Integer.parseInt(f.readLine());

            // pre-populate first digit in sequence
            final char[] sequence = new char[2 * N - 1];
            sequence[0] = Character.forDigit(1, 10);

            // solve
            final List<char[]> sequences = new ArrayList<>();
            makeSequence(sequences, sequence, 1, 0, N);

            // output result (only sequences that compute to 0)
            for (char[] result : sequences) {
                if (compute(result) == 0) {
                    out.println(printSequence(result));
                }
            }
        }
    }

    private static void makeSequence(List<char[]> results, char[] sequence, int digit, int i, int N) {
        // sequence of N digits plus operators between digits = 2*N-1; plus -1 for 0 index
        if (i == ((2 * N - 1) - 1)) {
            // reached maximum sequence size; add to list
            results.add(Arrays.copyOf(sequence, sequence.length));
            return;
        }

        sequence[++i] = ' ';
        sequence[++i] = Character.forDigit(++digit, 10);
        makeSequence(results, sequence, digit, i, N);
        i -= 2;
        sequence[++i] = '+';
        sequence[++i] = Character.forDigit(digit, 10);
        makeSequence(results, sequence, digit, i, N);
        i -= 2;
        sequence[++i] = '-';
        sequence[++i] = Character.forDigit(digit, 10);
        makeSequence(results, sequence, digit, i, N);
    }

    private static int compute(char[] sequence) {
        int result = 0;
        char op = '+';
        for (int i = 0; i < sequence.length; i++) {
            if (i == 0) {
                // first digit
                result = Character.digit(sequence[i], 10);
            } else if (i % 2 != 0) {
                // operator
                op = sequence[i];
                if (op == ' ') {
                    result *= 10;
                    result += Character.digit(sequence[++i], 10);
                }
            } else if (i % 2 == 0) {
                // for other digits, look ahead (if not on last digit) and
                // combine digits if next operator is ' '
                if ((i != (sequence.length - 1)) && sequence[i + 1] == ' ') {
                    int num1 = Character.digit(sequence[i], 10);
                    i += 2; // advance operator to next digit
                    num1 *= 10;
                    num1 += Character.digit(sequence[i], 10);
                    if (op == '+') {
                        result += num1;
                    } else if (op == '-') {
                        result -= num1;
                    }
                } else if (op == '+') {
                    result += Character.digit(sequence[i], 10);
                } else if (op == '-') {
                    result -= Character.digit(sequence[i], 10);
                }
            }
        }
        return result;
    }

    private static String printSequence(char[] sequence) {
        final StringBuilder builder = new StringBuilder();
        for (char character : sequence) {
            builder.append(character);
        }
        return builder.toString();
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/zerosum.java){:target="_blank" rel="noopener"}
