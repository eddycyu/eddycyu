---
layout: default
title: "Friday The Thirteenth"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: usaco
tags: [usaco]
order: 1.2.3
---

##### Note: description is copied from [USACO](http://www.usaco.org/){:target="_blank" rel="noopener"} training website and converted to markdown

### Description:
Is Friday the 13th really an unusual event?

That is, does the 13th of the month land on a Friday less often than on any 
other day of the week? To answer this question, write a program that will 
compute the frequency that the 13th of each month lands on Sunday, Monday, 
Tuesday, Wednesday, Thursday, Friday, and Saturday over a given period of 
N years. The time period to test will be from January 1, 1900 to December 
31, 1900+N-1 for a given number of years, N. N is positive and will not 
exceed 400.

**Note that the start year is NINETEEN HUNDRED, not NINETEEN NINETY.**

There are few facts you need to know before you can solve this problem:
* January 1, 1900 was on a Monday.
* Thirty days has September, April, June, and November, all the rest 
  have 31 except for February which has 28 except in leap years when it has 29.
* Every year evenly divisible by 4 is a leap year (1992 = 4*498 so 1992 will 
  be a leap year, but the year 1990 is not a leap year)
* The rule above does not hold for century years. Century years divisible by 
  400 are leap years, all others are not. Thus, the century years 1700, 1800, 
  1900 and 2100 are not leap years, but 2000 is a leap year.

Do not use any built-in date functions in your computer language.

Don't just precompute the answers, either, please.

##### PROGRAM NAME: friday

##### INPUT FORMAT
One line with the integer N.

##### SAMPLE INPUT (file friday.in)
```
20
```

##### OUTPUT FORMAT
Seven space separated integers on one line. These integers represent the number
of times the 13th falls on Saturday, Sunday, Monday, Tuesday, ..., Friday.

##### SAMPLE OUTPUT (file friday.out)
```
36 33 34 33 35 35 34
```

### Analysis:
Follow the facts and compute the answer. Define some constants and helper 
functions to make the code more readable.
    
### Solution:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class friday {

    // months
    private static final int JANUARY = 0;
    private static final int FEBRUARY = 1;
    private static final int MARCH = 2;
    private static final int APRIL = 3;
    private static final int MAY = 4;
    private static final int JUNE = 5;
    private static final int JULY = 6;
    private static final int AUGUST = 7;
    private static final int SEPTEMBER = 8;
    private static final int OCTOBER = 9;
    private static final int NOVEMBER = 10;
    private static final int DECEMBER = 11;

    // days of week
    private static final int MONDAY = 0;
    private static final int TUESDAY = 1;
    private static final int WEDNESDAY = 2;
    private static final int THURSDAY = 3;
    private static final int FRIDAY = 4;
    private static final int SATURDAY = 5;
    private static final int SUNDAY = 6;

    // number of days in a week
    private static final int DAYS_IN_WEEK = 7;

    private boolean isLeapYear(int year) {
        // special case for century year
        if (year % 100 == 0) {
            return (year % 400 == 0);
        }

        // check if year is divisible by 4
        return (year % 4 == 0);
    }

    private int getDaysInMonth(int month, int year) {
        switch (month) {
            case APRIL:
            case JUNE:
            case SEPTEMBER:
            case NOVEMBER:
                return 30;

            case JANUARY:
            case MARCH:
            case MAY:
            case JULY:
            case AUGUST:
            case OCTOBER:
            case DECEMBER:
                return 31;

            case FEBRUARY: {
                if (isLeapYear(year)) {
                    return 29;
                } else {
                    return 28;
                }
            }
            default:
                return 0;
        }
    }

    private int getDayOfWeek(int startingDayOfWeek, int daysToIncrement) {
        // 0 = monday ... 6 = sunday
        return (startingDayOfWeek + (daysToIncrement - 1)) % DAYS_IN_WEEK;
    }

    public static void main(String[] args) throws IOException {
        try (final BufferedReader f = new BufferedReader(new FileReader("friday.in"));
             final PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("friday.out")))) {

            // index 0 = monday ... 6 = sunday
            final int[] days = new int[7];

            // parse the number of years
            final int N = Integer.parseInt(f.readLine());

            // instantiate app
            final friday app = new friday();

            // calculate
            int dayOfWeekForFirst = 0;
            for (int year = 1900; year < (1900 + N); year++) {
                for (int month = 0; month < 12; month++) {
                    int daysInMonth = app.getDaysInMonth(month, year);
                    int dayOfWeekForThirteen = app.getDayOfWeek(dayOfWeekForFirst, 13); // advance to the 13th day
                    days[dayOfWeekForThirteen] += 1;

                    // advance to first day of next month
                    dayOfWeekForFirst = app.getDayOfWeek(dayOfWeekForFirst, daysInMonth + 1); // +1 for 1st day of new month
                }
            }

            // output
            out.print(days[SATURDAY] + " ");
            out.print(days[SUNDAY] + " ");
            out.print(days[MONDAY] + " ");
            out.print(days[TUESDAY] + " ");
            out.print(days[WEDNESDAY] + " ");
            out.print(days[THURSDAY] + " ");
            out.println(days[FRIDAY]);
        }
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/usaco/blob/master/src/friday.java){:target="_blank" rel="noopener"}
