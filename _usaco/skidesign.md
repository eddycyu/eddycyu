---
layout: default
title: "Ski Course Design"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.4.6
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Farmer John has N hills on his farm (1 <= N <= 1,000), each with an integer 
elevation in the range 0 .. 100. In the winter, since there is abundant 
snow on these hills, FJ routinely operates a ski training camp.

Unfortunately, FJ has just found out about a new tax that will be assessed 
next year on farms used as ski training camps. Upon careful reading of the 
law, however, he discovers that the official definition of a ski camp 
requires the difference between the highest and lowest hill on his property 
to be strictly larger than 17. Therefore, if he shortens his tallest hills 
and adds mass to increase the height of his shorter hills, FJ can avoid 
paying the tax as long as the new difference between the highest and lowest 
hill is at most 17.

If it costs x^2 units of money to change the height of a hill by x units, 
what is the minimum amount of money FJ will need to pay? FJ can change the 
height of a hill only once, so the total cost for each hill is the square 
of the difference between its original and final height. FJ is only willing 
to change the height of each hill by an integer amount.

##### PROGRAM NAME: skidesign

##### INPUT FORMAT

Line 1:       | The integer N.
--------------|---------------------
Lines 2..1+N: | Each line contains the elevation of a single hill.

##### SAMPLE INPUT (file skidesign.in)
```
5
20
4
1
24
21
```

##### INPUT DETAILS
FJ's farm has 5 hills, with elevations 1, 4, 20, 21, and 24.

##### OUTPUT FORMAT
The minimum amount FJ needs to pay to modify the elevations of his hills so the 
difference between largest and smallest is at most 17 units.

##### SAMPLE OUTPUT (file skidesign.out)
```
18
```

##### OUTPUT DETAILS
FJ keeps the hills of heights 4, 20, and 21 as they are. He adds mass to the 
hill of height 1, bringing it to height 4 (cost = 3^2 = 9). He shortens the 
hill of height 24 to height 21, also at a cost of 3^2 = 9.

### Analysis:
We can use a [brute force](https://en.wikipedia.org/wiki/Brute-force_search){:target="_blank" rel="noopener"} 
approach to solve this problem.

For each pair of hills with elevation difference > 17, compute the cost to make the elevation
changes. Keep track of the lowest cost.
    
### Solution:
```java
public class skidesign {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("skidesign.in"));
             final Scanner scanner = new Scanner(f);
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("skidesign.out")))) {

            // read input
            final int hillCount = scanner.nextInt();
            final int[] hills = new int[hillCount];
            for (int i = 0; i < hillCount; i++) {
                hills[i] = scanner.nextInt();
            }

            int minimumAmount = Integer.MAX_VALUE;
            if (hillCount == 1) {
                // nothing to do; there's only one hill!
                minimumAmount = 0;
            } else {
                // sort the hills by elevation (ascending order)
                Arrays.sort(hills);

                // iterate through the hills to find two hills with elevation difference > 17;
                // compute the cost to make the changes; keep track of the cheapest cost
                int changeCount = 0;
                for (int i = 0; i < hillCount - 1; i++) {
                    int highestElevationMax = -1;
                    int lowestElevationMin = -1;
                    for (int j = i + 1; j < hillCount; j++) {
                        int amount = 0;
                        final int lowestElevation = hills[i];
                        final int highestElevation = hills[j];
                        int diff = highestElevation - lowestElevation;
                        if (diff > 17) {
                            changeCount++;
                            final int lowerChange = (diff - 17) / 2; // round down
                            final int higherChange = (int) Math.ceil((diff - 17.0) / 2.0); // round up
                            highestElevationMax = hills[j] - higherChange;
                            lowestElevationMin = hills[i] + lowerChange;
                            // compute cost for change
                            for (int k = 0; k < hillCount; k++) {
                                if (hills[k] < lowestElevationMin) {
                                    final int change = lowestElevationMin - hills[k];
                                    amount += (change * change);
                                } else if (hills[k] > highestElevationMax) {
                                    final int change = hills[k] - highestElevationMax;
                                    amount += (change * change);
                                }
                                // speed optimization to eliminate unnecessary work
                                if (amount >= minimumAmount) {
                                    break; // no need to calculate any further for this loop
                                }
                            }
                            // keep track of the cheapest cost
                            minimumAmount = Math.min(minimumAmount, amount);
                        }
                    }
                }
                if (changeCount == 0) {
                    // all hills are within 17
                    minimumAmount = 0;
                }
            }

            // output result
            out.println(minimumAmount);
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/usaco/blob/master/src/skidesign.java){:target="_blank" rel="noopener"}
