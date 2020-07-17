---
layout: default
title: "How To Implement The Singleton Design Pattern"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, design pattern, singleton, creational design pattern]
author: Eddy Yu
published: true
---

This recipe shows how to implement the [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern){:target="_blank" rel="noopener"}
design pattern. This design pattern can be used when you need to restrict 
the instantiation of a class to a single instance, such as when implementing
logging, caching, thread pooling, configuration, etc.

The Singleton design pattern usually consists of:
* a private static variable to hold the only instance of the class
* a public static [factory](https://en.wikipedia.org/wiki/Factory_(object-oriented_programming)){:target="_blank" rel="noopener"} 
  method to return the single instance of the class
* a private constructor so it cannot be instantiated other than through the factory method

We will explore four different approaches to implementing this design pattern. 

* Option 1: eager initialization
* Option 2: [lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization){:target="_blank" rel="noopener"}
* Option 3: [initialization-on-demand holder](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom){:target="_blank" rel="noopener"}
* Option 4: single-element enum type

Each approach comes with its own set of pros and cons. Your choice of approach
will depend upon your specific 
[requirements](https://en.wikipedia.org/wiki/Requirement){:target="_blank" rel="noopener"}
and [use cases](https://en.wikipedia.org/wiki/Use_case){:target="_blank" rel="noopener"}; 
however, the preferred approach in most cases is option 4.

### Option 1: Eager Initialization
The first example shows how to implement the Singleton design pattern using
eager initialization with a public static factory method.

Pros:
* thread safe

Cons:
* eager initialization (if object creation is heavy))
* requires additional serialization code
* non-instantiation via constructor can be bypassed through reflection

```java
public class SingletonEager implements Serializable {

    private static final long serialVersionUID = 1L;

    // eager initialization
    private static SingletonEager instance = new SingletonEager();

    // hide the constructor
    private SingletonEager() {
    }

    // factory method to access single instance
    public static SingletonEager getInstance() {
        return instance;
    }

    // required in order to preserve singleton property for "serde"
    private Object readResolve() {
        return getInstance();
    }

    // other methods...
    public String doSomething() {
        return "do something";
    }

    public static void main(String[] args) throws Exception {
        final SingletonEager instance1 = SingletonEager.getInstance();
        final SingletonEager instance2 = SingletonEager.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that "serde" works properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonEager instance3 = (SingletonEager) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonEager.java){:target="_blank" rel="noopener"}

### Option 1 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```

### Option 2: Lazy Initialization
The second example shows how to implement the Singleton design pattern using 
lazy initialization with a public static factory method. It is designed for 
use in a single-threaded environment only.

Pros:
* lazy initialization

Cons:
* not thread safe
* requires additional serialization code
* non-instantiation via constructor can be bypassed through reflection

```java
public class SingletonLazy implements Serializable {

    private static final long serialVersionUID = 1L;
    private static SingletonLazy instance = null;

    // hide the constructor
    private SingletonLazy() {
    }

    // factory method to access single instance
    public static SingletonLazy getInstance() {
        // lazy initialization (not thread safe)
        if (instance == null) {
            instance = new SingletonLazy();
        }
        return instance;
    }

    // required in order to preserve singleton property for "serde"
    private Object readResolve() {
        return getInstance();
    }

    // other methods...
    public String doSomething() {
        return "do something";
    }

    public static void main(String[] args) throws Exception {
        final SingletonLazy instance1 = SingletonLazy.getInstance();
        final SingletonLazy instance2 = SingletonLazy.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that "serde" works properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonLazy instance3 = (SingletonLazy) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonLazy.java){:target="_blank" rel="noopener"}

### Option 2 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```

### Option 3: Initialization-On-Demand Holder
The third example shows how to implement the Singleton design pattern following
the initialization-on-demand holder idiom with a public static factory method. 
It uses lazy initialization and is thread safe.

Pros:
* lazy initialization
* thread safe

Cons:
* requires additional serialization code
* non-instantiation via constructor can be bypassed through reflection

```java
public class SingletonHolder implements Serializable {

    private static final long serialVersionUID = 1L;

    private static class LazyHolder {
        static final SingletonHolder INSTANCE = new SingletonHolder();
    }

    // hide the constructor
    private SingletonHolder() {
    }

    // factory method to access single instance
    public static SingletonHolder getInstance() {
        // lazy initialization
        return LazyHolder.INSTANCE;
    }

    // required in order to preserve singleton property for "serde"
    private Object readResolve() {
        return getInstance();
    }

    // other methods...
    public String doSomething() {
        return "do something";
    }

    public static void main(String[] args) throws Exception {
        final SingletonHolder instance1 = SingletonHolder.getInstance();
        final SingletonHolder instance2 = SingletonHolder.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that "serde" works properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonHolder instance3 = (SingletonHolder) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonHolder.java){:target="_blank" rel="noopener"}

### Option 3 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```

### Option 4: Single-Element Enum Type (preferred)
The final example shows how to implement the Singleton design pattern using a
single-element _Enum_ type, which provides a lazy initialization, thread-safe 
and serializable implementation.

Pros:
* lazy initialization
* thread safe
* inherently serializable (no additional code required)

Cons:
* none

```java
public enum SingletonEnum {
    INSTANCE;

    // other methods...
    public String doSomething() {
        return "do something";
    }

    public static void main(String[] args) throws Exception {
        final SingletonEnum instance1 = SingletonEnum.INSTANCE;
        final SingletonEnum instance2 = SingletonEnum.INSTANCE;
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that "serde" works properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonEnum instance3 = (SingletonEnum) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link To: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonEnum.java){:target="_blank" rel="noopener"}

### Option 4 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```