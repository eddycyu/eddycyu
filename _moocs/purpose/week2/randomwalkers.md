---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 2: Conditionals And Loops - Random Walkers"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.2.4
---

Write a program RandomWalkers.java that takes two integer command-line 
arguments _r_ and _trials_. In each of _trials_ independent 
experiments, simulate a random walk until the random walker is at 
Manhattan distance _r_ from the starting point. Print the average 
number of steps. 

```
~/Desktop/loops> java RandomWalkers 5 1000000
average number of steps = 14.98188

~/Desktop/loops> java RandomWalkers 5 1000000
average number of steps = 14.93918

~/Desktop/loops> java RandomWalkers 10 100000
average number of steps = 59.37386

~/Desktop/loops> java RandomWalkers 20 100000
average number of steps = 235.6288

~/Desktop/loops> java RandomWalkers 40 100000
average number of steps = 949.14712

~/Desktop/loops> java RandomWalkers 80 100000
average number of steps = 3775.7152

~/Desktop/loops> java RandomWalkers 160 100000
average number of steps = 15113.61108
```

As _r_ increases, we expect the random walker to take more and more steps. 
But how many more steps? Use RandomWalkers.java to formulate a hypothesis 
as to how the average number of steps grows as a function of _r_.

Estimating an unknown quantity by generating random samples and aggregating 
the results is an example of Monte Carlo simulation — a powerful 
computational technique that is used widely in statistical physics, 
computational finance, and computer graphics. 

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/loops/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class RandomWalkers {

    public static void main(String[] args) {
        final int r = Integer.parseInt(args[0]);
        final int trials = Integer.parseInt(args[1]);

        double random;
        int totalSteps = 0;
        for (int trial = 0; trial < trials; trial++) {
            int x = 0;
            int y = 0;
            int steps = 0;
            int distance = 0;
            while ((r > 0) && (distance != r)) {
                random = Math.random();
                if (random < 0.25) {
                    x -= 1;
                } else if (random < 0.50) {
                    x += 1;
                } else if (random < 0.75) {
                    y -= 1;
                } else {
                    y += 1;
                }
                steps++;

                // calculate new manhattan distance
                distance = Math.abs(x) + Math.abs(y);
            }
            totalSteps += steps;
        }
        System.out.println("average number of steps = " + (double) totalSteps / (double) trials);
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/RandomWalkers.java){:target="_blank" rel="noopener"}
