---
layout: default
title: "How To Use Bounded Wildcards (Java Generics)"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, generics, wildcard]
author: Eddy Yu
published: true
---

This recipe shows how to use bounded wildcards (Java Generics) to implement a 
_concat()_ method which will append the elements from list2 to list1. The type
of elements contained in list1 and list2 are of different types, but both types 
are subclasses of the elements contained in the combined list.

### Example:
```java
public class WildcardList<E> {

    private static <E> List<E> concat(List<? extends E> list1,
                                      List<? extends E> list2) {
        final List<E> result = new ArrayList<>(list1);
        result.addAll(list2);
        return result;
    }

    public static void main(String[] args) {
        final List<Integer> integers = Arrays.asList(1, 2, 3);
        final List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3, 4.4);
        final List<String> strings = Arrays.asList("one", "two", "three");

        // combine integers and doubles into a numbers list
        final List<Number> numbers = concat(integers, doubles);

        // combine numbers and strings into an objects list
        final List<Object> objects = concat(numbers, strings);

        // output
        System.out.print("numbers list: ");
        for (Number number : numbers) {
            System.out.print(number + " ");
        }
        System.out.println();
        System.out.print("objects list: ");
        for (Object object : objects) {
            System.out.print(object + " ");
        }
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/generics/WildcardList.java){:target="_blank" rel="noopener"}

### Output:
```
numbers list: 1 2 3 1.1 2.2 3.3 4.4 
objects list: 1 2 3 1.1 2.2 3.3 4.4 one two three
```