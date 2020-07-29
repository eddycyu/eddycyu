---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 10: Programming Languages - Bar Data Type"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.10.1
---

A bar aggregates related information (_name_, _value_, and _category_) for use 
in a bar chart. For example, the first bar drawn in the bar chart represents 
_name_ = Beijing, _value_ = 672, and _category_ = East Asia. In addition to 
methods for accessing the individual fields, you will need to sort bars in 
order of value. Implement the following API:

```java
public class Bar implements Comparable<Bar> {

    // Creates a new bar.
    public Bar(String name, int value, String category)

    // Returns the name of this bar.
    public String getName()

    // Returns the value of this bar.
    public int getValue()

    // Returns the category of this bar.
    public String getCategory()

    // Compare two bars by value.
    public int compareTo(Bar that)

    // Sample client (see below).
    public static void main(String[] args)
}
```

_Sorting arrays of objects._ To sort an array of objects, use Java's 
[Arrays.sort()](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-java.lang.Object:A-){:target="_blank" rel="noopener"}
method. For example, the following code fragment creates an array of 10 bars 
and sorts them in ascending order of value.
```java
// create an array of 10 bars
Bar[] bars = new Bar[10];
bars[0] = new Bar("Beijing",     22674, "East Asia");
bars[1] = new Bar("Cairo",       19850, "Middle East");
bars[2] = new Bar("Delhi",       27890, "South Asia");
bars[3] = new Bar("Dhaka",       19633, "South Asia");
bars[4] = new Bar("Mexico City", 21520, "Latin America");
bars[5] = new Bar("Mumbai",      22120, "South Asia");
bars[6] = new Bar("Osaka",       20409, "East Asia");
bars[7] = new Bar("São Paulo",   21698, "Latin America");
bars[8] = new Bar("Shanghai",    25779, "East Asia");
bars[9] = new Bar("Tokyo",       38194, "East Asia");

// sort in ascending order by weight
Arrays.sort(bars);
```

_Comparable interface._ In order to use a data type with _Arrays.sort()_, that 
data type must be comparable. This is Java's mechanism for specifying a total 
order among objects of a given type. To make a data type comparable, you must 
implement the [Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html){:target="_blank" rel="noopener"}
interface, which involves doing two things:
* Add _implements Comparable\<Bar\>_ to the class definition to indicate that 
  _Bar_ objects can be compared (by value).
* Include a method _compareTo()_ to specify how to compare two _Bar_ objects. 
  In this case, return a { negative integer, zero, positive integer } if value 
  of the invoking object is { less than, equal to, greater than } the value of 
  the argument object.
  
_Corner cases._ Handle invalid argument in the following manner:
* Throw an _IllegalArgumentException_ in the constructor if _name_ is _null_, 
  _value_ is negative, or _category_ is _null_.
* Throw a _NullPointerException_ if the argument to _compareTo()_ is _null_.

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/barchart/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class Bar implements Comparable<Bar> {

    private final String name;
    private final int value;
    private final String category;

    // Creates a new bar.
    public Bar(String name, int value, String category) {
        if ((name == null) || (value < 0) || (category == null)) {
            throw new IllegalArgumentException();
        }
        this.name = name;
        this.value = value;
        this.category = category;
    }

    // Returns the name of this bar.
    public String getName() {
        return name;
    }

    // Returns the value of this bar.
    public int getValue() {
        return value;
    }

    // Returns the category of this bar.
    public String getCategory() {
        return category;
    }

    // Compare two bars by value.
    public int compareTo(Bar that) {
        if (that == null) {
            throw new NullPointerException();
        }
        // sort value in ascending order
        return Integer.compare(value, that.value);
    }

    // Sample client (see below).
    public static void main(String[] args) {
        Bar[] bars = new Bar[3];
        bars[0] = new Bar("bar1", 10, "category1");
        bars[1] = new Bar("bar2", 5, "category1");
        bars[2] = new Bar("bar3", 10, "category1");
        Arrays.sort(bars);
        for (Bar bar : bars) {
            StdOut.println(bar.getName() + ", " + bar.getValue() + ", " + bar.getCategory());
        }
    }
}
``` 
Link To: [Java Source Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/Bar.java){:target="_blank" rel="noopener"}
