---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 9: Creating Data Types - Clock Data Type"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.9.2
---

Write a data type Clock.java that represents time on a 24-hour clock, such as 
00:00, 13:30, or 23:59. Time is measured in hours (00–23) and minutes (00–59). 
To do so, implement the following public API:

```java
public class Clock {

    // Creates a clock whose initial time is h hours and m minutes.
    public Clock(int h, int m)

    // Creates a clock whose initial time is specified as a string, using the format HH:MM.
    public Clock(String s)

    // Returns a string representation of this clock, using the format HH:MM.
    public String toString()

    // Is the time on this clock earlier than the time on that one?
    public boolean isEarlierThan(Clock that)

    // Adds 1 minute to the time on this clock.
    public void tic()

    // Adds Δ minutes to the time on this clock.
    public void toc(int delta)

    // Test client (see below).
    public static void main(String[] args)
}
```

Here is some more information about the required behavior:
* _Two-argument constructor._ Throw an _IllegalArgumentException_ if either 
  integer argument is outside its prescribed bounds (hours between 0 and 23, 
  minutes between 0 and 59).
* _One-argument constructor._ The string argument is composed of two digits, 
  followed by a colon, followed by two digits, such as 09:45. Throw an 
  _IllegalArgumentException_ if either the string argument is not in this 
  format or if it does not correspond to a valid time between 00:00 and 23:59.
* _String representation._ The format is the hours (2 digits), followed by a 
  colon, followed by the minutes (2 digits). Two examples are 00:00 and 23:59.
* _Ordering._ Times are ordered from 00:00 (earliest) to 23:59 (latest).
* _Tic._ Add one minute to the current time. For example, one minute after 
  06:00 is 06:01; one minute after 23:59 is 00:00.
* _Toc._ Add Δ minutes to the current time. For example, 60 minutes after 12:34 
  is 13:34. Throw an _IllegalArgumentException_ if Δ is negative.
* _Test client._ The _main()_ method must call each instance method directly 
  and help verify that they work as prescribed.
* _Performance._ All instance methods must take constant time.

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/oop2/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class Clock {

    private static final int MINUTES_PER_HOUR = 60;
    private static final int HOURS_PER_DAY = 24;
    private static final int MAX_HOURS = HOURS_PER_DAY - 1;
    private static final int MAX_MINUTES = MINUTES_PER_HOUR - 1;

    private int hours;      // 00-23
    private int minutes;    // 00-59

    // Creates a clock whose initial time is h hours and m minutes.
    public Clock(int h, int m) {
        if ((h < 0) || (h > MAX_HOURS)) {
            throw new IllegalArgumentException();
        }
        if ((m < 0) || (m > MAX_MINUTES)) {
            throw new IllegalArgumentException();
        }
        hours = h;
        minutes = m;
    }

    // Creates a clock whose initial time is specified as a string, using the format HH:MM.
    public Clock(String s) {
        if ((s == null) || (s.length() != 5) || (s.charAt(2) != ':')) {
            throw new IllegalArgumentException();
        }
        final int h = Integer.parseInt(s.substring(0, 2));
        final int m = Integer.parseInt(s.substring(3, 5));
        if ((h < 0) || (h > MAX_HOURS)) {
            throw new IllegalArgumentException();
        }
        if ((m < 0) || (m > MAX_MINUTES)) {
            throw new IllegalArgumentException();
        }
        hours = h;
        minutes = m;
    }

    // Returns a string representation of this clock, using the format HH:MM.
    public String toString() {
        return String.format("%02d:%02d", hours, minutes);
    }

    // Is the time on this clock earlier than the time on that one?
    public boolean isEarlierThan(Clock that) {
        if (hours < that.hours) {
            return true;
        } else if (hours > that.hours) {
            return false;
        } else {
            return (minutes < that.minutes);
        }
    }

    // Adds 1 minute to the time on this clock.
    public void tic() {
        minutes++;
        if (minutes > MAX_MINUTES) {
            hours++;
            minutes = 0;
        }
        if (hours > MAX_HOURS) {
            hours = 0;
        }
    }

    // Adds Δ minutes to the time on this clock.
    public void toc(int delta) {
        if (delta < 0) {
            throw new IllegalArgumentException();
        }
        if (delta > MAX_MINUTES) {
            final int dHours = (delta / MINUTES_PER_HOUR) % HOURS_PER_DAY;
            for (int i = 0; i < dHours; i++) {
                hours++;
                if (hours > MAX_HOURS) {
                    hours = 0;
                }
            }
        }
        final int dMinutes = delta % MINUTES_PER_HOUR;
        for (int i = 0; i < dMinutes; i++) {
            minutes++;
            if (minutes > MAX_MINUTES) {
                hours++;
                minutes = 0;
            }
            if (hours > MAX_HOURS) {
                hours = 0;
            }
        }
    }

    // Test client (see below).
    public static void main(String[] args) {
        final Clock clock1 = new Clock(0, 0);
        final Clock clock2 = new Clock(1, 30);
        StdOut.println("clock1: " + clock1);
        StdOut.println("clock2: " + clock2);
        StdOut.println("clock1 is before clock2: " + clock1.isEarlierThan(clock2));
        clock1.tic();
        StdOut.println("clock1: " + clock1);
        StdOut.println("clock2: " + clock2);
        clock1.toc(90);
        StdOut.println("clock1: " + clock1);
        StdOut.println("clock2: " + clock2);
        StdOut.println("clock1 is before clock2: " + clock1.isEarlierThan(clock2));
        Clock clock3 = new Clock("22:59");
        StdOut.println("clock3: " + clock3);
        clock3.toc(1440);
        StdOut.println("clock3: " + clock3);
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/Clock.java){:target="_blank" rel="noopener"}
