---
layout: default
title: "How To Install MariaDB Via Docker On Ubuntu 20.04 LTS"
date: 2020-07-03 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, mariadb, database, docker, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [MariaDB](https://mariadb.org/){:target="_blank" rel="noopener"}
10.5 via [Docker](https://www.docker.com/){:target="_blank" rel="noopener"} on
[Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS. 
These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation
* [Docker](/blog/how-to-install-docker-on-ubuntu-20.04-lts){:target="_blank" rel="noopener"}
  installed on system 

### Install MariaDB Via Docker
Search for image.
```
$ docker search mariadb
```

There are many mariadb images available. We are interested in the official image. Output:
```
$ docker search mariadb
NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mariadb                                MariaDB is a community-developed fork of MyS…   3565                [OK]                
... results truncated ...
```

Pull the image.
```
# pull the latest version
$ docker pull mariadb

# or pull a specific version
$ docker pull mariadb:10.5
```

Output:
```
$ docker pull mariadb
Using default tag: latest
latest: Pulling from library/mariadb
3ff22d22a855: Pull complete 
e7cb79d19722: Pull complete 
323d0d660b6a: Pull complete 
b7f616834fd0: Pull complete 
78ed0160f03e: Pull complete 
a122e9306ac4: Pull complete 
673e89352b19: Pull complete 
caf1e694359b: Pull complete 
04f5e4f6ead3: Pull complete 
a41772aadb3d: Pull complete 
c3811aa2fa0a: Pull complete 
655ad574d3c7: Pull complete 
90ae536d75f0: Pull complete 
Digest: sha256:812d3a450addcfe416420c72311798f3f3109a11d9677716dc631c429221880c
Status: Downloaded newer image for mariadb:latest
docker.io/library/mariadb:latest
```

Confirm the image download.
```
$ docker images
```

Output:
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mariadb             latest              8075b7694a2d        2 days ago          407MB
```

Create __config__ and __data__ directories on the host system. The mariadb 
container will use these directories for the configuration and data locations.
Place a __my.cnf__ with custom configuration values into the __config__ 
directory to override desired configuration values coming from the container.
```
$ mkdir -p ~/mariadb/config
$ mkdir -p ~/mariadb/data
```

Launch the mariadb container with container port 3306 remapped to host port 
33061, the configuration directory in the container remapped to the host 
configuration directory (__~/mariadb/config__), and the data directory in the
container remapped to the host data directory (__~/mariadb/data__). 

Replace __mymariadb__ with the desired container name, __33061__ with the 
desired host port, __strong_password__ with the desired root password, and 
__testdb__ with the desired database name.
```
$ docker run -d --name mymariadb \
    -p 33061:3306 \
    -v ~/mariadb/config:/etc/mysql/conf.d \
    -v ~/mariadb/data:/var/lib/mysql \
    -e MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    -e MYSQL_DATABASE=testdb \
    mariadb
```

Verify that the container is running.
```
$ docker ps -a
```

Output:
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                     NAMES
7e3a1c2f743e        mariadb             "docker-entrypoint.s…"   12 seconds ago      Up 10 seconds       0.0.0.0:33061->3306/tcp   mymariadb
```

View the container log to verify that the database started properly.
```
$ docker logs mymariadb
```

Output:
```
2020-07-26 22:30:57+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 1:10.5.4+maria~focal started.
2020-07-26 22:30:57+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-07-26 22:30:57+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 1:10.5.4+maria~focal started.
2020-07-26 22:30:58+00:00 [Note] [Entrypoint]: Initializing database files


PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:

'/usr/bin/mysqladmin' -u root password 'new-password'
'/usr/bin/mysqladmin' -u root -h 1424236ba365 password 'new-password'

Alternatively you can run:
'/usr/bin/mysql_secure_installation'

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the MariaDB Knowledgebase at https://mariadb.com/kb or the
MySQL manual for more instructions.

Please report any problems at https://mariadb.org/jira

The latest information about MariaDB is available at https://mariadb.org/.
You can find additional information about the MySQL part at:
https://dev.mysql.com
Consider joining MariaDB's strong and vibrant community:
https://mariadb.org/get-involved/

2020-07-26 22:31:00+00:00 [Note] [Entrypoint]: Database files initialized
2020-07-26 22:31:00+00:00 [Note] [Entrypoint]: Starting temporary server
2020-07-26 22:31:00+00:00 [Note] [Entrypoint]: Waiting for server startup
2020-07-26 22:31:00 0 [Note] mysqld (mysqld 10.5.4-MariaDB-1:10.5.4+maria~focal) starting as process 100 ...
2020-07-26 22:31:00 0 [Note] InnoDB: Using Linux native AIO
2020-07-26 22:31:00 0 [Note] InnoDB: Uses event mutexes
2020-07-26 22:31:00 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2020-07-26 22:31:00 0 [Note] InnoDB: Number of pools: 1
2020-07-26 22:31:00 0 [Note] InnoDB: Using SSE4.2 crc32 instructions
2020-07-26 22:31:00 0 [Note] mysqld: O_TMPFILE is not supported on /tmp (disabling future attempts)
2020-07-26 22:31:00 0 [Note] InnoDB: Initializing buffer pool, total size = 134217728, chunk size = 134217728
2020-07-26 22:31:00 0 [Note] InnoDB: Completed initialization of buffer pool
2020-07-26 22:31:00 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
2020-07-26 22:31:00 0 [Note] InnoDB: 128 rollback segments are active.
2020-07-26 22:31:00 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2020-07-26 22:31:00 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2020-07-26 22:31:00 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2020-07-26 22:31:00 0 [Note] InnoDB: 10.5.4 started; log sequence number 45041; transaction id 21
2020-07-26 22:31:00 0 [Note] Plugin 'FEEDBACK' is disabled.
2020-07-26 22:31:00 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2020-07-26 22:31:00 0 [Note] InnoDB: Buffer pool(s) load completed at 200726 22:31:00
2020-07-26 22:31:00 0 [Note] Reading of all Master_info entries succeeded
2020-07-26 22:31:00 0 [Note] Added new Master_info '' to hash table
2020-07-26 22:31:00 0 [Note] mysqld: ready for connections.
Version: '10.5.4-MariaDB-1:10.5.4+maria~focal'  socket: '/run/mysqld/mysqld.sock'  port: 0  mariadb.org binary distribution
2020-07-26 22:31:01+00:00 [Note] [Entrypoint]: Temporary server started.
Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/leapseconds' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/tzdata.zi' as time zone. Skipping it.
2020-07-26 22:31:06+00:00 [Note] [Entrypoint]: Creating database testdb

2020-07-26 22:31:06+00:00 [Note] [Entrypoint]: Stopping temporary server
2020-07-26 22:31:06 0 [Note] mysqld (initiated by: root[root] @ localhost []): Normal shutdown
2020-07-26 22:31:06 0 [Note] Event Scheduler: Purging the queue. 0 events
2020-07-26 22:31:06 0 [Note] InnoDB: FTS optimize thread exiting.
2020-07-26 22:31:06 0 [Note] InnoDB: Starting shutdown...
2020-07-26 22:31:06 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool
2020-07-26 22:31:06 0 [Note] InnoDB: Buffer pool(s) dump completed at 200726 22:31:06
2020-07-26 22:31:07 0 [Note] InnoDB: Shutdown completed; log sequence number 45081; transaction id 24
2020-07-26 22:31:07 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
2020-07-26 22:31:07 0 [Note] mysqld: Shutdown complete

2020-07-26 22:31:08+00:00 [Note] [Entrypoint]: Temporary server stopped

2020-07-26 22:31:08+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

2020-07-26 22:31:08 0 [Note] mysqld (mysqld 10.5.4-MariaDB-1:10.5.4+maria~focal) starting as process 1 ...
2020-07-26 22:31:08 0 [Note] InnoDB: Using Linux native AIO
2020-07-26 22:31:08 0 [Note] InnoDB: Uses event mutexes
2020-07-26 22:31:08 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2020-07-26 22:31:08 0 [Note] InnoDB: Number of pools: 1
2020-07-26 22:31:08 0 [Note] InnoDB: Using SSE4.2 crc32 instructions
2020-07-26 22:31:08 0 [Note] mysqld: O_TMPFILE is not supported on /tmp (disabling future attempts)
2020-07-26 22:31:08 0 [Note] InnoDB: Initializing buffer pool, total size = 134217728, chunk size = 134217728
2020-07-26 22:31:08 0 [Note] InnoDB: Completed initialization of buffer pool
2020-07-26 22:31:08 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
2020-07-26 22:31:08 0 [Note] InnoDB: 128 rollback segments are active.
2020-07-26 22:31:08 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2020-07-26 22:31:08 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2020-07-26 22:31:08 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2020-07-26 22:31:08 0 [Note] InnoDB: 10.5.4 started; log sequence number 45081; transaction id 21
2020-07-26 22:31:08 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
2020-07-26 22:31:08 0 [Note] Plugin 'FEEDBACK' is disabled.
2020-07-26 22:31:08 0 [Note] InnoDB: Buffer pool(s) load completed at 200726 22:31:08
2020-07-26 22:31:08 0 [Note] Server socket created on IP: '::'.
2020-07-26 22:31:08 0 [Note] Reading of all Master_info entries succeeded
2020-07-26 22:31:08 0 [Note] Added new Master_info '' to hash table
2020-07-26 22:31:08 0 [Note] mysqld: ready for connections.
Version: '10.5.4-MariaDB-1:10.5.4+maria~focal'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
```

Verify that the host data directory is being used.
```
ls -l ~/mariadb/data
```

Output:
```
$ ls -l ~/mariadb/data
total 141016
-rw-rw---- 1 systemd-coredump systemd-coredump  18546688 Jul 26 15:31 aria_log.00000001
-rw-rw---- 1 systemd-coredump systemd-coredump        52 Jul 26 15:31 aria_log_control
-rw-rw---- 1 systemd-coredump systemd-coredump       976 Jul 26 15:31 ib_buffer_pool
-rw-rw---- 1 systemd-coredump systemd-coredump  12582912 Jul 26 15:31 ibdata1
-rw-rw---- 1 systemd-coredump systemd-coredump 100663296 Jul 26 15:31 ib_logfile0
-rw-rw---- 1 systemd-coredump systemd-coredump  12582912 Jul 26 15:31 ibtmp1
-rw-rw---- 1 systemd-coredump systemd-coredump         0 Jul 26 15:31 multi-master.info
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:31 mysql
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:30 performance_schema
drwx------ 2 systemd-coredump systemd-coredump      4096 Jul 26 15:31 testdb
```

Log into the running container.
```
$ docker exec -it mymariadb /bin/bash
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
| testdb             |
+--------------------+
```

Secure the installation.
```
$ mysql_secure_installation
```

Press ENTER, N, N, and then Y for all the remaining 
questions. Output:
```
$ mysql_secure_installation

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

Create a dedicated user with root privileges that is secured by a strong 
password. Replace __admin__ and __strong_password__ with the desired username 
and password.
```
$ mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'strong_password' WITH GRANT OPTION;"
``` 

### Start Container
```
$ docker run mymariadb
```

### Stop Container
```
$ docker stop mymariadb
```

### Delete Container
The container must be stopped before removing.
```
$ docker rm mymariadb
```

### Delete Image
```
$ docker rmi mariadb
```
