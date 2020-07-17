---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 4: Functions - Activation Functions"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.5.1
---

Write a program ActivationFunction.java to compute various 
[activiation functions](https://en.wikipedia.org/wiki/Activation_function){:target="_blank" rel="noopener"} 
that arise in [neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network){:target="_blank" rel="noopener"}. 
An activation function is a function that maps real numbers into a desired 
range, such as between 0 and 1 or between –1 and +1.

* The _Heaviside step_ function is given by:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if x < 0<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;H(x) = 1/2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if x = 0<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if x > 0

* The _sigmoid_ function is given by: σ(x) = 1 / (1 + e<sup>-x</sup>)
* The _hyperbolic tangent_ function is given by: tanh(x) = (e<sup>x</sup> - e<sup>-x</sup>) / (e<sup>x</sup> + e<sup>-x</sup>)
* The _softsign_ function is given by: f(x) = x / (1 + abs(x))
* The _square nonlinearity_ function is given by:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if x <= -2<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SQNL(x) = x + (x<sup>2</sup>/4)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if -2 < x < 0<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x - (x<sup>2</sup>/4)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if 0 <= x < 2<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if x >=2
          
All activation functions should return NaN (not a number) if the argument is NaN.

To do so, organize your program according to the following public API:
```java
public class ActivationFunction {

    // Returns the Heaviside function of x.
    public static double heaviside(double x)

    // Returns the sigmoid function of x.
    public static double sigmoid(double x)

    // Returns the hyperbolic tangent of x.
    public static double tanh(double x)

    // Returns the softsign function of x.
    public static double softsign(double x)

    // Returns the square nonlinearity function of x.
    public static double sqnl(double x)

    // Takes a double command-line argument x and prints each activation
    // function, evaluated, in the format (and order) given below.
    public static void main(String[] args)
}
```

Here are some sample executions:

```
~/Desktop/functions> java ActivationFunction 0.0
heaviside(0.0) = 0.5
  sigmoid(0.0) = 0.5
     tanh(0.0) = 0.0
 softsign(0.0) = 0.0
     sqnl(0.0) = 0.0

~/Desktop/functions> java ActivationFunction 1.0
heaviside(1.0) = 1.0
  sigmoid(1.0) = 0.7310585786300049
     tanh(1.0) = 0.7615941559557649
 softsign(1.0) = 0.5
     sqnl(1.0) = 0.75

~/Desktop/functions> java ActivationFunction -0.5
heaviside(-0.5) = 0.0
  sigmoid(-0.5) = 0.3775406687981454
     tanh(-0.5) = -0.4621171572600098
 softsign(-0.5) = -0.3333333333333333
     sqnl(-0.5) = -0.4375
```

##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/functions/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class ActivationFunction {

    // Returns the Heaviside function of x.
    public static double heaviside(double x) {
        if (Double.isNaN(x)) {
            return Double.NaN;
        }
        if (x < 0.0) {
            return 0.0;
        } else if (x == 0.0) {
            return 0.5;
        } else {
            return 1.0;
        }
    }

    // Returns the sigmoid function of x.
    public static double sigmoid(double x) {
        return 1.0 / (1.0 + Math.exp(-x));
    }

    // Returns the hyperbolic tangent of x.
    public static double tanh(double x) {
        if ((x == Double.POSITIVE_INFINITY) || (x == Double.MAX_VALUE)) {
            return 1.0;
        } else if ((x == Double.NEGATIVE_INFINITY) || (x == -Double.MAX_VALUE)) {
            return -1.0;
        }
        return (Math.exp(x) - Math.exp(-x)) / (Math.exp(x) + Math.exp(-x));
    }

    // Returns the softsign function of x.
    public static double softsign(double x) {
        if (x == Double.POSITIVE_INFINITY) {
            return 1.0;
        } else if (x == Double.NEGATIVE_INFINITY) {
            return -1.0;
        }
        return x / (1.0 + Math.abs(x));
    }

    // Returns the square nonlinearity function of x.
    public static double sqnl(double x) {
        if (Double.isNaN(x)) {
            return Double.NaN;
        }
        if (x <= -2.0) {
            return -1.0;
        } else if ((x > -2.0) && (x < 0.0)) {
            return x + (Math.pow(x, 2) / 4);
        } else if ((x >= 0.0) && (x < 2)) {
            return x - (Math.pow(x, 2) / 4);
        } else {
            return 1.0;
        }
    }

    public static void main(String[] args) {
        double x = Double.parseDouble(args[0]);
        StdOut.println("heaviside(" + x + ") = " + heaviside(x));
        StdOut.println("  sigmoid(" + x + ") = " + sigmoid(x));
        StdOut.println("     tanh(" + x + ") = " + tanh(x));
        StdOut.println(" softsign(" + x + ") = " + softsign(x));
        StdOut.println("     sqnl(" + x + ") = " + sqnl(x));
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/ActivationFunction.java){:target="_blank" rel="noopener"}
