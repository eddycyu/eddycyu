---
layout: default
title: "Mixing Milk"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.4.1
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
The Merry Milk Makers company buys milk from farmers, packages it into 
attractive 1- and 2-Unit bottles, and then sells that milk to grocery 
stores so we can each start our day with delicious cereal and milk.

Since milk packaging is such a difficult business in which to make money, 
it is important to keep the costs as low as possible. Help Merry Milk 
Makers purchase the farmers' milk in the cheapest possible manner. The MMM 
company has an extraordinarily talented marketing department and knows 
precisely how much milk they need each day to package for their customers.

The company has contracts with several farmers from whom they may purchase 
milk, and each farmer has a (potentially) different price at which they sell 
milk to the packing plant. Of course, a herd of cows can only produce so much 
milk each day, so the farmers already know how much milk they will have 
available.

Each day, Merry Milk Makers can purchase an integer number of units of milk 
from each farmer, a number that is always less than or equal to the farmer's 
limit (and might be the entire production from that farmer, none of the 
production, or any integer in between).

Given:

* The Merry Milk Makers' daily requirement of milk
* The cost per unit for milk from each farmer
* The amount of milk available from each farmer

calculate the minimum amount of money that Merry Milk Makers must spend to meet 
their daily need for milk.

Note: The total milk produced per day by the farmers will always be sufficient 
to meet the demands of the Merry Milk Makers even if the prices are high.

##### PROGRAM NAME: milk

##### INPUT FORMAT

Line 1:              | Two integers, N and M.<br/>The first value, N, (0 <= N <= 2,000,000) is the amount of milk that Merry Milk Makers wants per day.<br/>The second, M, (0 <= M <= 5,000) is the number of farmers that they may buy from.
---------------------|---------------------
Lines 2 through M+1: | The next M lines each contain two integers: P<sub>i</sub> and A<sub>i</sub>.<br/>P<sub>i</sub> (0 <= P<sub>i</sub> <= 1,000) is price in cents that farmer i charges.<br/>A<sub>i</sub> (0 <= A<sub>i</sub> <= 2,000,000) is the amount of milk that farmer i can sell to Merry Milk Makers per day.

##### SAMPLE INPUT (file milk.in)
```
100 5
5 20
9 40
3 10
8 80
6 30
```

##### INPUT EXPLANATION
100 5 -- MMM wants 100 units of milk from 5 farmers

5 20  -- Farmer 1 says, "I can sell you 20 units at 5 cents per unit"

9 40     etc.

3 10  -- Farmer 3 says, "I can sell you 10 units at 3 cents per unit"

8 80     etc.

6 30  -- Farmer 5 says, "I can sell you 30 units at 6 cents per unit"

##### OUTPUT FORMAT
A single line with a single integer that is the minimum cost that Merry Milk 
Makers must pay for one day's milk.

##### SAMPLE OUTPUT (file milk.out)
```
630
```
##### OUTPUT EXPLANATION
Here's how the MMM company spent only 630 cents to purchase 100 units of milk:
```
Price per Unit   Units Available   Units bought   Price * # units   Total cost   Notes
5                20                20             5*20              100
9                40                0                                             Bought no milk from farmer 2
3                10                10             3*10              30
8                80                40             8*40              320          Did not buy all 80 units!
6                30                30             6*30              180
Total            180               10                               630          Cheapest total cost
```

### Analysis:
We can use a [greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm){:target="_blank" rel="noopener"} 
to solve this problem.

First sort the farmers by price (ascending order). As we iterate through
each farmer, purchase as much milk as available (and needed) from each
farmer. Continue until we have purchased as much as we need.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class milk {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("milk.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("milk.out")))) {

            String line = f.readLine();
            String[] tokens = line.split(" ");
            int unitsToBuy = Integer.parseInt(tokens[0]);
            final int farmersCount = Integer.parseInt(tokens[1]);

            // get the farmers
            final List<Farmer> farmers = new ArrayList<>();
            for (int i = 0; i < farmersCount; i++) {
                line = f.readLine();
                tokens = line.split(" ");
                final int pricePerUnit = Integer.parseInt(tokens[0]);
                final int unitsToSell = Integer.parseInt(tokens[1]);
                farmers.add(new Farmer(pricePerUnit, unitsToSell));
            }

            // sort the farmers by price
            Collections.sort(farmers);

            // find the lowest cost for the desired quantity
            int totalCost = 0;
            for (Farmer farmer : farmers) {
                if (farmer.units <= unitsToBuy) {
                    // buy all available units and find more
                    totalCost += farmer.units * farmer.price;
                    unitsToBuy -= farmer.units;
                } else {
                    // buy some units and stop
                    totalCost += unitsToBuy * farmer.price;
                    break;
                }
            }

            // output cost
            out.println(totalCost);
        }
    }

    static class Farmer implements Comparable<Farmer> {
        int price = 0;
        int units = 0;

        Farmer(int price, int units) {
            super();
            this.price = price;
            this.units = units;
        }

        @Override
        public int compareTo(Farmer other) {
            return (this.price - other.price);
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/milk.java){:target="_blank" rel="noopener"}
