---
layout: default
title: "How To Install Mysql On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, mysql, database, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [Mysql](https://www.mysql.com/){:target="_blank" rel="noopener"}
8.0 on [Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation

### Install MariaDB
First update and upgrade the installed software packages on the system.
```
$ sudo apt update && sudo apt upgrade -y
```

Fetch and install Mysql from the repository.
```
$ sudo apt install mysql-server
```

Verify that the database is installed and running.
```
$ mysql -V
$ sudo systemctl status mysql
```

Output:
```
$ mysql -V
mysql  Ver 8.0.20-0ubuntu0.20.04.1 for Linux on x86_64 ((Ubuntu))

$ sudo systemctl status mysql
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2020-07-3 00:00:30 PDT; 10h ago
   Main PID: 3065 (mysqld)
     Status: "Server is operational"
      Tasks: 39 (limit: 4624)
     Memory: 327.7M
     CGroup: /system.slice/mysql.service
             └─3065 /usr/sbin/mysqld
```

Secure the installation.
```
$ sudo mysql_secure_installation
```

Follow the prompts. Output:
```
$ sudo mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No: Y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 2
Please set the password for root here.

New password: 

Re-enter new password: 

Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : Y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : Y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : Y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : Y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y
Success.

All done!
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
mysql> CREATE USER 'admin'@'localhost' IDENTIFIED BY 'strong_password';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
mysql> EXIT
``` 

### Auto Start Database
Enable Mysql to automatically start with the system.
```
$ sudo systemctl enable mysql
```

### Manually Start Database
```
$ sudo systemctl start mysql
```

### Manually Stop Database
```
$ sudo systemctl stop mysql
```
