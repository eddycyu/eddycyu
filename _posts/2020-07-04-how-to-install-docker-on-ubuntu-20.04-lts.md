---
layout: default
title: "How To Install Docker On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, docker, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [Docker](https://www.docker.com/){:target="_blank" rel="noopener"}
on [Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation

### Install Docker
First update and upgrade the installed software packages on the system.
```
$ sudo apt update && sudo apt upgrade -y
```

Install a few prerequisite packages which let APT use packages over HTTPS. 
```
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Add the GPG key for the official Docker repository to your system.
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the Docker repository to APT sources.
```
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

Update the package database with the Docker packages from the newly added repository.
```
$ sudo apt update
```

Fetch and install Docker.
```
$ sudo apt install docker-ce
```

Add user to the docker group to avoid having to type __sudo__ before every 
__docker__ command.
```
$ sudo usermod -a -G docker $USER
```

To apply the new group membership, type the following:
```
$ su - ${USER}
```

Verify that Docker is installed and running.
```
   $ docker -v
   $ sudo systemctl status docker
```
   
Output:
```
$ docker -v
Docker version 19.03.12, build 48a66213fe

$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2020-07-03 11:27:35 PDT; 58s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 9752 (dockerd)
      Tasks: 10
     Memory: 37.9M
     CGroup: /system.slice/docker.service
             └─9752 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

### Verify Installation

Download the __hello_world__ image and run it in a container to verify that 
Docker is working properly.
```
$ docker run hello-world
```

Output:
```
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Cleanup the test.
```
# remove the container
$ docker rm optimistic_rhodes

# remove the image
$ docker image rm hello-world
```
