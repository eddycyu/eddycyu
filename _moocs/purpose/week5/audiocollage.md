---
layout: default
title: "Coursera - Computer Science: Programming With A Purpose"
subtitle: "Week 4: Functions - Audio Collage"
date: 2020-07-01 12:00:00 -0700
author: Eddy Yu
category: moocs
tags: [moocs, coursera, programming with a purpose]
order: purpose.5.3
---

Create a library to manipulate digital audio and use that library to create an 
audio collage. As in lecture, we will represent sound as an array of real 
numbers between –1 and +1, with 44,100 samples per second. You will write a 
library of functions to produce audio effects by manipulating such arrays.

* _Amplify._ Crate a new sound that contains the same samples as an existing 
  sound, but with each sample multiplied by a constant α. This increases the 
  volume when α > 1 and decreases it when 0 < α < 1.
* _Reverse._ Create a new sound that contains the same samples as an existing 
  sound, but in reverse order. This can lead to unexpected and entertaining 
  results.
* _Merge/join._ Create a new sound that combines two existing sounds by 
  appending the second one after the first. If the two sounds have _m_ and 
  _n_ samples, then the resulting sound has _m + n_ samples. This enables you 
  to play two sounds sequentially.
* _Mix/overlay._ Create a new sound that combines two existing sounds by 
  summing the values of the corresponding samples. If one sound is longer than 
  the other, append 0s to the shorter sound before summing. This enables you 
  to play two sounds simultaneously.
* _Change speed._ Create a new sound that changes the duration of an existing 
  sound via resampling. If the existing sound has _n_ samples, then the new 
  sound has ⌊_n_ / α⌋ samples for some constant α > 0, with sample _i_ of the new 
  sound having the same amplitude as sample ⌊_i_ α⌋ of the existing sound.
  
To do so, organize your program according to the following public API:  

```java
public class AudioCollage {

    // Returns a new array that rescales a[] by a multiplicative factor of alpha.
    public static double[] amplify(double[] a, double alpha)

    // Returns a new array that is the reverse of a[].
    public static double[] reverse(double[] a)

    // Returns a new array that is the concatenation of a[] and b[].
    public static double[] merge(double[] a, double[] b)

    // Returns a new array that is the sum of a[] and b[],
    // padding the shorter arrays with trailing 0s if necessary.
    public static double[] mix(double[] a, double[] b)

    // Returns a new array that changes the speed by the given factor.
    public static double[] changeSpeed(double[] a, double alpha)

    // Creates an audio collage and plays it on standard audio.
    // See below for the requirements.
    public static void main(String[] args)
}
```

Here is some more information about the required behavior:

* The functions must not mutate the argument array(s).
* You may assume that the array arguments are not null and that α > 0 in 
  _changeSpeed()_.
* The _main()_ method must create an audio collage and play it using 
  _StdAudio.play()_.
  * Do not use any command-line arguments.
  * The duration must be between 10 and 60 seconds (441,000 to 2,646,000 samples).
  * All samples sent to standard audio must be between –1 and +1.
  * It must use at least five different WAV files. Several WAV files are provided:
  [beatbox.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [buzzer.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [chimes.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [cow.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [dialup.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [exposure.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [harp.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [piano.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [scratch.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"},
  [silence.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"}, and
  [singer.wav](https://github.com/eddycyu/programming-with-a-purpose/blob/master/data/beatbox.wav){:target="_blank" rel="noopener"}.
  You may also supply or create your own.
  * Use _StdAudio.read()_ to read a WAV file and extract its samples as an 
    array of floating-point numbers between –1 and +1.
  * It must use all of the audio effects specified in the API (amplify, 
    reverse, merge, mix, and change speed). Feel free to add additional audio 
    effects (see the FAQ for some ideas).
  * Be creative!
  
##### Note: the above description is copied from [Coursera](https://coursera.cs.princeton.edu/introcs/assignments/functions/specification.php){:target="_blank" rel="noopener"} and converted to markdown for convenience

### Solution:
```java
public class AudioCollage {

    // Returns a new array that rescales a[] by a multiplicative factor of alpha.
    public static double[] amplify(double[] a, double alpha) {
        double[] result = new double[a.length];
        for (int i = 0; i < result.length; i++) {
            result[i] = a[i] * alpha;
        }
        return result;
    }

    // Returns a new array that is the reverse of a[].
    public static double[] reverse(double[] a) {
        double[] result = new double[a.length];
        for (int i = 0; i < a.length; i++) {
            result[a.length - 1 - i] = a[i];
        }
        return result;
    }

    // Returns a new array that is the concatenation of a[] and b[].
    public static double[] merge(double[] a, double[] b) {
        double[] result = new double[a.length + b.length];
        for (int i = 0; i < a.length; i++) {
            result[i] = a[i];
        }
        for (int i = 0; i < b.length; i++) {
            result[a.length + i] = b[i];
        }
        return result;
    }

    // Returns a new array that is the sum of a[] and b[],
    // padding the shorter arrays with trailing 0s if necessary.
    public static double[] mix(double[] a, double[] b) {
        final int length = Math.max(a.length, b.length);
        double[] result = new double[length];
        for (int i = 0; i < result.length; i++) {
            if ((i < a.length) && (i < b.length)) {
                result[i] = a[i] + b[i];
            } else if ((i < a.length) && (i >= b.length)) {
                result[i] = a[i];
            } else {
                result[i] = b[i];
            }
        }
        return result;
    }

    // Returns a new array that changes the speed by the given factor.
    public static double[] changeSpeed(double[] a, double alpha) {
        double[] result = new double[(int) (a.length / alpha)];
        for (int i = 0; i < result.length; i++) {
            result[i] = a[(int) (i * alpha)];
        }
        return result;
    }

    // Creates an audio collage and plays it on standard audio.
    // See below for the requirements.
    public static void main(String[] args) {
        final double[] a = StdAudio.read("beatbox.wav");
        final double[] b = StdAudio.read("harp.wav");
        final double[] c = StdAudio.read("piano.wav");
        final double[] d = StdAudio.read("exposure.wav");
        final double[] e = StdAudio.read("singer.wav");
        final double[] aMixB = mix(a, b);
        final double[] dMixE = mix(d, e);
        final double[] reverseC = reverse(c);
        final double[] amplifyE = amplify(e, 1.5);
        final double[] speedD = changeSpeed(d, 2);
        double[] result = merge(aMixB, dMixE);
        result = merge(result, reverseC);
        result = merge(result, amplifyE);
        result = merge(result, speedD);

        // sample must be between -1 and 1
        for (int i = 0; i < result.length; i++) {
            if (result[i] > 1.0) {
                result[i] = 0.9999999999;
            } else if (result[i] < -1.0) {
                result[i] = -0.9999999999;
            }
        }
        StdAudio.play(result);
    }
}
``` 
Link: [Java Code](https://github.com/eddycyu/programming-with-a-purpose/blob/master/src/AudioCollage.java){:target="_blank" rel="noopener"}
