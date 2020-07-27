---
layout: default
title: "How To Install MariaDB On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, mariadb, database, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [MariaDB](https://mariadb.org/){:target="_blank" rel="noopener"}
10.5 on [Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation

### Install MariaDB
First update and upgrade the installed software packages on the system.
```
$ sudo apt update && sudo apt upgrade -y
```

Add the repository for MariaDB 10.5 and import MariaDB's GPG public key.
```
$ sudo apt install software-properties-common
$ sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
$ sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.nodesdirect.com/mariadb/repo/10.5/ubuntu focal main'
``` 

Fetch and install MariaDB 10.5 from the repository.
```
$ sudo apt install mariadb-server
```

Verify that the database is installed and running.
```
$ mysql -V
$ sudo systemctl status mariadb
```

Secure the installation.
```
$ sudo mysql_secure_installation
```

Press ENTER, N, N, and then Y for all the remaining 
questions. Output:
```
$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] N
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] N
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

Login as __root__ and create a dedicated __admin__ account that is secured by
a password.
```
$ sudo mysql
```

From within the shell, create a dedicated user with root privileges that is 
secured by a strong password. Replace __admin__ and __strong_password__ with 
the desired username and password.
```
MariaDB [(none)] > GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'strong_password' WITH GRANT OPTION;
MariaDB [(none)] > FLUSH PRIVILEGES;
MariaDB [(none)] > EXIT
``` 

### Auto Start Database
Enable MariaDB to automatically start with the system.
```
$ sudo systemctl enable mariadb
```

### Manually Start Database
```
$ sudo systemctl start mariadb
```

### Manually Stop Database
```
$ sudo systemctl stop mariadb
```
