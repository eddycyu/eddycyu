---
layout: default
title: "How To Implement The Decorator Design Pattern"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, design pattern, decorator, structural design pattern]
author: Eddy Yu
published: true
---

This recipe shows how to implement the [Decorator](https://en.wikipedia.org/wiki/Decorator_pattern){:target="_blank" rel="noopener"}
design pattern. This design pattern can be used when you need to add additional
functionality to an individual object as opposed to a class of objects. It is
especially useful when dealing with requirements that involve the composition 
of objects from a large set of options. With the Decorator design pattern,
these compositions can be done dynamically in real-time.

### Example:
Assume a hypothetical cafe that sells coffee and tea. Various options can be
added to each drink, such as sugar, milk, tapioca pearls, etc. As the number
of options grow, the number of combinations of these add-ons will grow 
exponentially. If you were to design a system to represent all the possible 
drinks with add-on combinations, you may want to avoid using 
[inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)){:target="_blank" rel="noopener"},
as you may end up with too many subclasses to manage. Rather, you may want
to consider using the Decorator design pattern which leverages 
[composition](https://en.wikipedia.org/wiki/Object_composition){:target="_blank" rel="noopener"}
and could significantly reduce the number of classes created.

We start by defining an interface for Drink. All concrete drinks (e.g. coffee,
tea) will implement this interface.
```java
interface Drink {
    String getName();
    double getPrice();
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Drink.java){:target="_blank" rel="noopener"}

We can then define the concrete drink classes as follows.
```java
class Coffee implements Drink {
    @Override
    public String getName() {
        return "Coffee (+$4)";
    }

    @Override
    public double getPrice() {
        return 4.0;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Coffee.java){:target="_blank" rel="noopener"}
```java
class Tea implements Drink {
    @Override
    public String getName() {
        return "Tea (+$3)";
    }

    @Override
    public double getPrice() {
        return 3.0;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Tea.java){:target="_blank" rel="noopener"}

Next, we define an abstract class for add-on decorators. All concrete 
add-on decorators (e.g. sugar, milk, tapioca, etc.) will extend this 
abstract class and override the _getName()_ and _getPrice()_ methods. 
```java
abstract class AddOnDecorator implements Drink {

    // reference to object that is being decorated
    protected final Drink drink;

    public AddOnDecorator(Drink drink) {
        this.drink = drink;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/AddOnDecorator.java){:target="_blank" rel="noopener"}

We can then define the concrete add-on decorator classes as follows.
```java
public class Sugar extends AddOnDecorator {
    public Sugar(Drink drink) {
        super(drink);
    }

    @Override
    public String getName() {
        return drink.getName() + " with sugar (+$0.10)";
    }

    @Override
    public double getPrice() {
        return drink.getPrice() + 0.1;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Sugar.java){:target="_blank" rel="noopener"}

```java
public class Milk extends AddOnDecorator {
    public Milk(Drink drink) {
        super(drink);
    }

    @Override
    public String getName() {
        return drink.getName() + " with milk (+$0.10)";
    }

    @Override
    public double getPrice() {
        return drink.getPrice() + 0.1;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Milk.java){:target="_blank" rel="noopener"}

```java
public class Tapioca extends AddOnDecorator {
    public Tapioca(Drink drink) {
        super(drink);
    }

    @Override
    public String getName() {
        return drink.getName() + " with tapioca pearls (+$0.50)";
    }

    @Override
    public double getPrice() {
        return drink.getPrice() + 0.5;
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Tapioca.java){:target="_blank" rel="noopener"}

To demonstrate how these classes work, we use the following driver class.
```java
public class Cafe {
    public static void main(String[] args) {
        final NumberFormat formatter = NumberFormat.getCurrencyInstance(new Locale("en", "US"));

        // plain coffee
        Drink coffee = new Coffee();
        System.out.println(coffee.getName() + ": " + formatter.format(coffee.getPrice()));

        // coffee with milk
        coffee = new Milk(coffee);
        System.out.println(coffee.getName() + ": " + formatter.format(coffee.getPrice()));

        // coffee with milk and sugar
        coffee = new Sugar(coffee);
        System.out.println(coffee.getName() + ": " + formatter.format(coffee.getPrice()));

        // plain tea
        Drink tea = new Tea();
        System.out.println(tea.getName() + ": " + formatter.format(tea.getPrice()));

        // tea with sugar
        tea = new Sugar(tea);
        System.out.println(tea.getName() + ": " + formatter.format(tea.getPrice()));

        // tea with sugar and tapioca pearls
        tea = new Tapioca(tea);
        System.out.println(tea.getName() + ": " + formatter.format(tea.getPrice()));
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/decorator/Cafe.java){:target="_blank" rel="noopener"}

### Example Output:
```
Coffee (+$4): $4.00
Coffee (+$4) with milk (+$0.10): $4.10
Coffee (+$4) with milk (+$0.10) with sugar (+$0.10): $4.20
Tea (+$3): $3.00
Tea (+$3) with sugar (+$0.10): $3.10
Tea (+$3) with sugar (+$0.10) with tapioca pearls (+$0.50): $3.60
```