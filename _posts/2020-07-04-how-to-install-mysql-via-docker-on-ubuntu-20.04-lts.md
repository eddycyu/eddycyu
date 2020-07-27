---
layout: default
title: "How To Install Mysql Via Docker On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, mysql, database, docker, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [Mysql](https://www.mysql.com/){:target="_blank" rel="noopener"}
8.0 via [Docker](https://www.docker.com/){:target="_blank" rel="noopener"} on
[Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation
* [Docker](/blog/how-to-install-docker-on-ubuntu-20.04-lts){:target="_blank" rel="noopener"}
  installed on system 

### Install Mysql Via Docker
Search for image.
```
$ docker search mysql
```

There are many mysql images available. We are interested in the official image. Output:
```
$ docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   9767                [OK]                
... results truncated ...
```

Pull the image.
```
# pull the latest version
$ docker pull mysql

# or pull a specific version
$ docker pull mysql:8.0
```

Output:
```
$ docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
6ec8c9369e08: Pull complete 
177e5de89054: Pull complete 
ab6ccb86eb40: Pull complete 
e1ee78841235: Pull complete 
09cd86ccee56: Pull complete 
78bea0594a44: Pull complete 
caf5f529ae89: Pull complete 
cf0fc09f046d: Pull complete 
4ccd5b05a8f6: Pull complete 
76d29d8de5d4: Pull complete 
8077a91f5d16: Pull complete 
922753e827ec: Pull complete 
Digest: sha256:fb6a6a26111ba75f9e8487db639bc5721d4431beba4cd668a4e922b8f8b14acc
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

Confirm the image download.
```
$ docker images
```

Output:
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               latest              e3fcc9e1cc04        4 days ago          544MB
```

Create __config__ and __data__ directories on the host system. The mysql 
container will use these directories for the configuration and data locations.
Place a __my.cnf__ with custom configuration values into the __config__ 
directory to override desired configuration values coming from the container.
```
$ mkdir -p ~/mysql/config
$ mkdir -p ~/mysql/data
```

Launch the mysql container with container port 3306 remapped to host port 
33061, the configuration directory in the container remapped to the host 
configuration directory (__~/mysql/config__), and the data directory in the
container remapped to the host data directory (__~/mysql/data__). 

Replace __mymysql__ with the desired container name, __33061__ with the 
desired host port, __strong_password__ with the desired root password, and 
__testdb__ with the desired database name.
```
$ docker run -d --name mymysql \
    -p 33061:3306 \
    -v ~/mysql/config:/etc/mysql/conf.d \
    -v ~/mysql/data:/var/lib/mysql \
    -e MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    -e MYSQL_DATABASE=testdb \
    mysql
```

Verify that the container is running.
```
$ docker ps -a
```

Output:
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                NAMES
303d0f78e3bb        mysql               "docker-entrypoint.s…"   12 seconds ago      Up 11 seconds       33060/tcp, 0.0.0.0:33061->3306/tcp   mymysql
```

View the container log to verify that the database started properly.
```
$ docker logs mymysql
```

Output:
```
2020-07-26 23:06:01+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-07-26 23:06:01+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-07-26 23:06:01+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-07-26 23:06:01+00:00 [Note] [Entrypoint]: Initializing database files
2020-07-26T23:06:01.713729Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.21) initializing of server in progress as process 45
2020-07-26T23:06:01.718758Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2020-07-26T23:06:02.067890Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2020-07-26T23:06:03.374126Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
2020-07-26 23:06:06+00:00 [Note] [Entrypoint]: Database files initialized
2020-07-26 23:06:06+00:00 [Note] [Entrypoint]: Starting temporary server
2020-07-26T23:06:07.599197Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.21) starting as process 92
2020-07-26T23:06:07.673403Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2020-07-26T23:06:08.412008Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2020-07-26T23:06:08.681268Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: /var/run/mysqld/mysqlx.sock
2020-07-26T23:06:09.023008Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2020-07-26T23:06:09.023199Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2020-07-26T23:06:09.024652Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2020-07-26T23:06:09.042107Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.21'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server - GPL.
2020-07-26 23:06:09+00:00 [Note] [Entrypoint]: Temporary server started.
Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
2020-07-26 23:06:12+00:00 [Note] [Entrypoint]: Creating database testdb

2020-07-26 23:06:12+00:00 [Note] [Entrypoint]: Stopping temporary server
2020-07-26T23:06:12.212719Z 11 [System] [MY-013172] [Server] Received SHUTDOWN from user root. Shutting down mysqld (Version: 8.0.21).
2020-07-26T23:06:14.260447Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.21)  MySQL Community Server - GPL.
2020-07-26 23:06:15+00:00 [Note] [Entrypoint]: Temporary server stopped

2020-07-26 23:06:15+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

2020-07-26T23:06:15.477240Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.21) starting as process 1
2020-07-26T23:06:15.485915Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2020-07-26T23:06:15.652120Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2020-07-26T23:06:15.758958Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
2020-07-26T23:06:15.815033Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2020-07-26T23:06:15.815340Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2020-07-26T23:06:15.817175Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2020-07-26T23:06:15.835929Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.21'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
```

Verify that the host data directory is being used.
```
ls -l ~/mysql/data
```

Output:
```
$ ls -l ~/mysql/data
total 128728
-rw-rw---- 1 systemd-coredump systemd-coredump  18546688 Jul 26 15:55 aria_log.00000001
-rw-rw---- 1 systemd-coredump systemd-coredump        52 Jul 26 15:55 aria_log_control
-rw-rw---- 1 systemd-coredump systemd-coredump       976 Jul 26 15:55 ib_buffer_pool
-rw-rw---- 1 systemd-coredump systemd-coredump  12582912 Jul 26 15:55 ibdata1
-rw-rw---- 1 systemd-coredump systemd-coredump 100663296 Jul 26 15:55 ib_logfile0
-rw-rw---- 1 systemd-coredump systemd-coredump         0 Jul 26 15:31 multi-master.info
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:31 mysql
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:30 performance_schema
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:31 testdb
```

Log into the running container.
```
$ docker exec -it mymysql /bin/bash
```

Verify that __testdb__ was created.
```
$ mysql -p -e "SHOW DATABASES;"
```

Output:
```
$ mysql -p -e "SHOW DATABASES;"
Enter password: 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| testdb             |
+--------------------+
```

Secure the installation.
```
$ mysql_secure_installation
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

Create a dedicated user with root privileges that is secured by a strong 
password. Replace __admin__ and __strong_password__ with the desired username 
and password.
```
$ mysql -p -e "CREATE USER 'admin'@'localhost' IDENTIFIED BY 'strong_password';"
$ mysql -p -e "GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;"
``` 

### Start Container
```
$ docker run mymysql
```

### Stop Container
```
$ docker stop mymysql
```

### Delete Container
The container must be stopped before removing.
```
$ docker rm mymysql
```

### Delete Image
```
$ docker rmi mymysql
```
