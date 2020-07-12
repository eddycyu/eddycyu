---
layout: default
title: "How To Implement The Singleton Design Pattern"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [java, design pattern, singleton]
author: Eddy Yu
published: true
---

This note shows how to implement the [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern){:target="_blank" rel="noopener"}
design pattern. This design pattern can be used when you need to restrict 
the instantiation of a class to a single instance.

We will explore three different approaches to implement the solution. The first
approach uses [lazy initialization](https://en.wikipedia.org/wiki/Lazy_initialization){:target="_blank" rel="noopener"}
to delay the creation of the instance until it is first requested, and is 
suitable only for single-thread environments. The second approach follows 
the [initialization-on-demand holder](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom){:target="_blank" rel="noopener"}
idiom and is multi-thread safe and uses lazy initialization. The final (and 
recommended) approach uses a single-element _Enum_ type and is multi-thread 
safe. The first two approaches require the addition of the _readResolve()_
method in order to support proper "serde" without breaking the singleton 
property.

### Example 1:
The first example uses lazy initialization and should only be used in single 
threaded environments.
```java
public class SingletonSingleThreaded implements Serializable {

    private static final long serialVersionUID = 1L;
    private static SingletonSingleThreaded instance = null;

    // hide the constructor
    private SingletonSingleThreaded() {
        super();
    }

    public static SingletonSingleThreaded getInstance() {
        // lazy initialization (not multi-thread safe)
        if (instance == null) {
            instance = new SingletonSingleThreaded();
        }
        return instance;
    }

    // required in order to preserve singleton property for serde
    private Object readResolve() {
        return getInstance();
    }

    public static void main(String[] args) throws Exception {
        final SingletonSingleThreaded instance1 = SingletonSingleThreaded.getInstance();
        final SingletonSingleThreaded instance2 = SingletonSingleThreaded.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that serde is working properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonSingleThreaded instance3 = (SingletonSingleThreaded) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonSingleThreaded.java){:target="_blank" rel="noopener"}

### Example 1 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```

### Example 2:
The second example uses the initialization-on-demand holder idiom to provide a 
multi-thread safe implementation with lazy initialization.
```java
public class SingletonMultiThreadedHolder implements Serializable {

    private static final long serialVersionUID = 1L;

    private static class LazyHolder {
        static final SingletonMultiThreadedHolder INSTANCE = new SingletonMultiThreadedHolder();
    }

    // hide the constructor
    private SingletonMultiThreadedHolder() {
        super();
    }

    public static SingletonMultiThreadedHolder getInstance() {
        return LazyHolder.INSTANCE;
    }

    // required in order to preserve singleton property for serde
    private Object readResolve() {
        return getInstance();
    }

    public static void main(String[] args) throws Exception {
        final SingletonMultiThreadedHolder instance1 = SingletonMultiThreadedHolder.getInstance();
        final SingletonMultiThreadedHolder instance2 = SingletonMultiThreadedHolder.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that serde is working properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonMultiThreadedHolder instance3 = (SingletonMultiThreadedHolder) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonMultiThreadedHolder.java){:target="_blank" rel="noopener"}

### Example 2 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```

### Example 3 (Recommended):
The third example uses a single-element _Enum_ type to provide a multi-thread safe 
implementation.
```java
public enum SingletonMultiThreadedEnum {
    INSTANCE;

    public static SingletonMultiThreadedEnum getInstance() {
        return INSTANCE;
    }

    public static void main(String[] args) throws Exception {
        final SingletonMultiThreadedEnum instance1 = SingletonMultiThreadedEnum.getInstance();
        final SingletonMultiThreadedEnum instance2 = SingletonMultiThreadedEnum.getInstance();
        if (instance1 == instance2) {
            System.out.println("instance1 and instance2 are the same instance");
        } else {
            System.out.println("instance1 and instance2 are different instances");
        }

        // verify that serde is working properly
        final ByteArrayOutputStream baos = new ByteArrayOutputStream();
        final ObjectOutputStream oos = new ObjectOutputStream(baos);
        oos.writeObject(instance1);
        oos.close();
        final InputStream is = new ByteArrayInputStream(baos.toByteArray());
        final ObjectInputStream ois = new ObjectInputStream(is);
        final SingletonMultiThreadedEnum instance3 = (SingletonMultiThreadedEnum) ois.readObject();
        if (instance1 == instance3) {
            System.out.println("instance1 and instance3 are the same instance");
        } else {
            System.out.println("instance1 and instance3 are different instances");
        }
        ois.close();
    }
}
```
Link: [Java Source Code](https://github.com/eddycyu/learnbyexample/blob/master/src/main/java/dev/eddycyu/designpattern/singleton/SingletonMultiThreadedEnum.java){:target="_blank" rel="noopener"}

### Example 3 Output:
```
instance1 and instance2 are the same instance
instance1 and instance3 are the same instance
```