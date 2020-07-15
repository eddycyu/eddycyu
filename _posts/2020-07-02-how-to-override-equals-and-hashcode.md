---
layout: default
title: "How To Override equals() And hashCode()"
date: 2020-07-02 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, equals, hashcode]
author: Eddy Yu
published: true
---

This recipe shows how to override the _equals()_ and _hashCode()_ methods in Java.

__WARNING:__ If you override _equals()_, you __must__ also override 
_hashCode()_. Failure to do so will prevent hash-based collections from working
properly.

### Example:
The following example uses a hypothetical _Person_ object with fields of 
various types to illustrate how to handle each type when implementing the 
overrides.
```java
public class Person {

    private String name;
    private char gender;
    private short age;
    private int zipcode;
    private long personalId;
    private float weight;
    private double netWorth;
    private boolean isRetired;
    private String[] activities;
    private List<Person> friends;

    public Person(String name, char gender, short age) {
        this.name = name;
        this.gender = gender;
        this.age = age;
    }

    // add other constructors, getters, setters, etc...
}
```
The _equals()_ method can be overridden as follows:
```java
    @Override
    public boolean equals(Object obj) {
        // check if the references are pointing to the same object
        if (obj == this) {
            return true;
        }

        // check if the objects are of the same type
        if (!(obj instanceof Person)) {
            return false;
        }

        // check if the significant values are the same
        final Person person = (Person) obj;
        return Objects.equals(person.name, name)
                && person.gender == gender
                && person.age == age
                && person.zipcode == zipcode
                && person.personalId == personalId
                && Float.compare(person.weight, weight) == 0
                && Double.compare(person.netWorth, netWorth) == 0
                && person.isRetired == isRetired
                && Arrays.equals(person.activities, activities)
                && Objects.equals(person.friends, friends);
    }
```
The easiest way to override the _hashCode()_ method is to use the platform 
library. Use this option only for cases where performance is not crucial.
```java
    @Override
    public int hashCode() {
        return Objects.hash(name, gender, age, zipcode, personalId,
                weight, netWorth, isRetired, activities, friends);
    }
``` 
For cases where performance is important, use a custom implementation for
_hashCode()_.
```java
    @Override
    public int hashCode() {
        // 31 is a good prime number to choose when generating a hash code
        int result = (name == null ? 0 : name.hashCode());
        result = 31 * result + Character.hashCode(gender);
        result = 31 * result + Short.hashCode(age);
        result = 31 * result + Integer.hashCode(zipcode);
        result = 31 * result + Long.hashCode(personalId);
        result = 31 * result + Float.hashCode(weight);
        result = 31 * result + Double.hashCode(netWorth);
        result = 31 * result + Boolean.hashCode(isRetired);
        result = 31 * result + Arrays.hashCode(activities);
        result = 31 * result + (friends == null ? 0 : friends.hashCode());
        return result;
    }
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/comparison/Person.java){:target="_blank" rel="noopener"}
