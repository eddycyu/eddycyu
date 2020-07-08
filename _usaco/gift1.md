---
layout: default
title: "Greedy Gift Givers"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.2.2
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
A group of NP (2 ≤ NP ≤ 10) uniquely named friends has decided to exchange 
gifts of money. Each of these friends might or might not give some money 
to some or all of the other friends (although some might be cheap and give
to no one). Likewise, each friend might or might not receive money from any
or all of the other friends. Your goal is to deduce how much more money each
person receives than they give.

The rules for gift-giving are potentially different than you might expect. 
Each person goes to the bank (or any other source of money) to get a certain 
amount of money to give and divides this money evenly among all those to 
whom he or she is giving a gift. No fractional money is available, so 
dividing 7 among 2 friends would be 3 each for the friends with 1 left over 
– that 1 left over goes into the giver's "account". All the participants' 
gift accounts start at 0 and are decreased by money given and increased by 
money received.

In any group of friends, some people are more giving than others (or at 
least may have more acquaintances) and some people have more money than 
others.

Given:
* a group of friends, no one of whom has a name longer than 14 characters,
* the money each person in the group spends on gifts, and
* a (sub)list of friends to whom each person gives gifts,
  determine how much money each person ends up with.

**Important Note**
The grader machine is a Linux machine that uses standard Unix conventions: 
end of line is a single character often known as '\n'. This differs from 
Windows, which ends lines with two characters, '\r\ and '\n'. Do not let 
your program get trapped by this!
 
##### PROGRAM NAME: gift1

##### INPUT FORMAT
Line #    | Contents
----------|---------
1         | A single integer, NP
2..NP+1   | Line i+1 contains the name of group member i
NP+2..end | NP groups of lines organized like this:<br/><br/>The first line of each group tells the person's name who will be giving gifts.<br/><br/>The second line in the group contains two numbers:<br/>- The amount of money (in the range 0..2000) to be divided into gifts by the giver<br/>- NG<sub>i</sub> (0 ≤ NG<sub>i</sub> ≤ NP), the number of people to whom the giver will give gifts<br/><br/>If NG<sub>i</sub> is nonzero, each of the next NG<sub>i</sub> lines lists the name of a recipient of a gift; recipients are not repeated in a single giver's list.

##### SAMPLE INPUT (file gift1.in)
```
5
dave
laura
owen
vick
amr
dave
200 3
laura
owen
vick
owen
500 1
dave
amr
150 2
vick
owen
laura
0 2
amr
vick
vick
0 0
```

##### OUTPUT FORMAT
The output is NP lines, each with the name of a person followed by a single 
blank followed by the net gain or loss (final_money_value - initial_money_value) 
for that person. The names should be printed in the same order they appear 
starting on line 2 of the input.

All gifts are integers. Each person gives the same integer amount of money to 
each friend to whom any money is given, and gives as much as possible that 
meets this constraint. Any money not given is kept by the giver.

##### SAMPLE OUTPUT (file gift1.out)
```
dave 302
laura 66
owen -359
vick 141
amr -150
```

##### OUTPUT EXPLANATION
Five names: dave, laura, owen, vick, amr. Let's keep a table of the gives 
(money) each person 'has':
```
    dave        laura       owen        vick        amr
    0           0           0           0           0
```
First, 'dave' splits 200 among 'laura', 'owen', and 'vick'. That comes to 66 
each, with 2 left over:
```
    -200+2      +66         +66         +66         0
→
    -198        66          66          66          0
```
Second, 'owen' gives 500 to 'dave':
```
    -198+500    66          66-500      66          0
→
    302	        66          -434        66          0
```
Third, 'amr' splits 150 between 'vick' and 'owen':
```
    302         66          -434+75     66+75       -150
→
    302         66          -359        141         -150
```
Fourth, 'laura' splits 0 between 'amr' and 'vick'; no changes:
```
    302         66          -359        141         -150
```
Finally, 'vick' gives 0 to no one:
```
    dave        laura       owen        vick        amr
    302         66          -359        141         -150
```
### Analysis:
This is a very straight forward problem. To help us keep track of the details,
we create a People object. Then, it's just a matter of following the gift-giving
rules and outputting the results.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class gift1 {

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("gift1.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("gift1.out")))) {

            // parse the number of people in the group
            final int NP = Integer.parseInt(f.readLine());

            // parse the people in the group
            final List<Person> group = new ArrayList<>(NP);
            final Map<String, Person> groupMap = new HashMap<>();
            for (int i = 0; i < NP; i++) {
                final String name = f.readLine();
                final Person person = new Person(name);
                group.add(person);
                groupMap.put(name, person);
            }

            // parse the details for each person in the group
            for (int i = 0; i < NP; i++) {
                final Person person = groupMap.get(f.readLine());
                final String line = f.readLine();
                final String[] tokens = line.split(" ");
                person.amountToGive = Integer.parseInt(tokens[0]);
                final int numberOfFriends = Integer.parseInt(tokens[1]);
                for (int j = 0; j < numberOfFriends; j++) {
                    person.addFriend(groupMap.get(f.readLine()));
                }
            }

            // exchange gifts
            for (Person person : group) {
                if (person.getNumberOfFriends() > 0) {
                    // distribute gifts to friends
                    final int giftAmount = person.amountToGive / person.getNumberOfFriends();
                    final int remainder = person.amountToGive % person.getNumberOfFriends();
                    person.distributeGifts(giftAmount);
                    person.addAccount(remainder);
                } else {
                    // no gifts to give; keep the money
                    person.addAccount(person.amountToGive);
                }
            }

            // output results
            for (Person person : group) {
                out.print(person.name);
                out.print(" ");
                out.println(person.account - person.amountToGive);
            }
        }
    }

    public static class Person {
        String name;
        int account;
        int amountToGive;
        final private List<Person> friends;

        private Person() {
            super();
            friends = new ArrayList<>();
        }

        Person(String name) {
            this();
            this.name = name;
        }

        void addAccount(int amount) {
            account += amount;
        }

        void addFriend(Person person) {
            friends.add(person);
        }

        int getNumberOfFriends() {
            return friends.size();
        }

        void distributeGifts(int giftAmount) {
            for (Person friend : friends) {
                friend.addAccount(giftAmount);
            }
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/gift1.java){:target="_blank" rel="noopener"}
