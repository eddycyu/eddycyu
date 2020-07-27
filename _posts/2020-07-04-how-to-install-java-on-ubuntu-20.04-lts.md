---
layout: default
title: "How To Install Java On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, java, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install Java (JRE/JDK) on 
[Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation

### Install JRE/JDK
First update and upgrade the installed software packages on the system.
```
$ sudo apt update && sudo apt upgrade -y
```

Check if Java is already installed:
```
# Java Runtime Environment (JRE)
$ java -version

# Java Development Kit (JDK)
$ javac -version
```

If Java cannot be found on the system, then install the desired version(s):
```
# Java 8 JRE/JDK
$ sudo apt install openjdk-8-jre openjdk-8-jdk

# Java 11 JRE/JDK
$ sudo apt install openjdk-11-jre openjdk-11-jdk
```

### Configure Default Java Version
If there are multiple versions of Java installed, you can configure which
version to use as the default.

Type the following to show the list of currently installed __java__ versions.
```
$ sudo update-alternatives --config java
```

You will see output such as:
```
$ sudo update-alternatives --config java
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

Input the number associated with the version to use as the default and press 
ENTER. 

Repeat the same steps for __javac__.
```
$ sudo update-alternatives --config javac
```

# Set Environment Variables

Add system-wide environment variables for _JRE_HOME_ and _JAVA_HOME_ in
__/etc/environment__.
```
$ sudo nano /etc/environment
```

At the end of the file, add the following lines:
```
JRE_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre"
JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
```

To apply the changes to your current session:
```
$ source /etc/environment
```

To verify that the environment variables are set properly, execute the 
following commands and confirm that the output matches the correct
paths. 
```
$ echo $JRE_HOME
$ echo $JAVA_HOME
```