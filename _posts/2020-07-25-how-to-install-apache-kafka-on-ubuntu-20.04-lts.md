---
layout: default
title: "How To Install Apache Kafka (Via Confluent) On Ubuntu 20.04 LTS"
date: 2020-07-25 12:00:00 -0700
author: Eddy Yu
category: blog
tags: [install, kafka, ubuntu]
author: Eddy Yu
published: true
---

This recipe shows how to install [Apache Kafka](https://kafka.apache.org/){:target="_blank" rel="noopener"}
on [Ubuntu](https://ubuntu.com/){:target="_blank" rel="noopener"} 20.04 LTS.
The installation will allow for the publishing and subscribing of Avro 
messages, ingesting data from a database to a Kafka topic, and exporting 
messages from a Kafka topic to a database.

These instructions are performed on a fresh Ubuntu Desktop/Server installation.

### Prerequisites
* Ubuntu 20.04 LTS installation
* [OpenJDK 8](/blog/how-to-install-java-on-ubuntu-20.04-lts){:target="_blank" rel="noopener"}
  installed on system 
* [MariaDB](/blog/how-to-install-mariadb-on-ubuntu-20.04-lts){:target="_blank" rel="noopener"}
  or [Mysql](/blog/how-to-install-mysql-on-ubuntu-20.04-lts){:target="_blank" rel="noopener"} 
  installed on system (for Kafka Connect)

### Update System
First update and upgrade the installed software packages on the system.
```
$ sudo apt update && sudo apt upgrade -y
```

### Install Utilities
Install some useful utilities that will be used later.
```
$ sudo apt install curl jq
```

### Create Dedicated User For Kafka
For security and to minimize risk, create a dedicated user for Kafka. This user 
will be used for running all Kafka related services (Zookeeper, Kafka, Schema
Registry, Kafka Connect).
```
$ sudo adduser --system --no-create-home --disabled-password --disabled-login kafka
```

### Install Confluent (Community)
Confluent provides a community package which includes convenient features such
as a schema registry and connectors. It also provides a bundled Zookeeper so 
you don't have to separately download and install it if you don't already have 
a deployed Zookeeper ensemble.
```
$ sudo mkdir /opt/confluent
$ wget https://packages.confluent.io/archive/5.5/confluent-community-5.5.1-2.12.tar.gz
$ sudo tar -xzvf confluent-community-5.5.1-2.12.tar.gz --directory /opt/confluent --strip-components 1
$ sudo mkdir -p /var/lib/zookeeper
$ sudo mkdir -p /var/lib/kafka-logs
$ sudo mkdir -p /var/lib/kafka-streams
$ sudo chown -R kafka:nogroup /opt/confluent
$ sudo chown -R kafka:nogroup /var/lib/zookeeper
$ sudo chown -R kafka:nogroup /var/lib/kafka-logs
$ sudo chown -R kafka:nogroup /var/lib/kafka-streams
```

### Install JDBC Driver (for Kafka Connect)
Kafka Connect enables the streaming of data between Apache Kafka and other data
systems. Connectors can be written to ingest data from a database to Kafka 
topics, or to export data from Kafka topics to batch systems such as Hadoop for 
offline processing.

To connect with MariaDB:
```
$ sudo wget https://downloads.mariadb.com/Connectors/java/connector-java-2.6.1/mariadb-java-client-2.6.1.jar \
    -P /opt/confluent/share/java/kafka-connect-jdbc 
$ sudo chown kafka:nogroup /opt/confluent/share/java/kafka-connect-jdbc/mariadb-java-client-*
```

To connect with Mysql:
```
$ sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.21.tar.gz
$ sudo tar -xzvf mysql-connector-java-8.0.21.tar.gz --directory /opt/confluent/share/java/kafka-connect-jdbc \
    mysql-connector-java-8.0.21/mysql-connector-java-8.0.21.jar --strip-components 1
$ sudo chown kafka:nogroup /opt/confluent/share/java/kafka-connect-jdbc/mysql-connector-java-*
```

### Set Environment Variables
Add system-wide environment variables by creating a __confluent-vars.sh__ in 
__/etc/profile.d__.
```
$ sudo nano /etc/profile.d/confluent-vars.sh
```

Add the following content to the file. Replace __\<ip.to.broker\>__ with the IP 
address to the kafka broker, and __\<ip.to.schema.registry\>__ with the IP 
address to the schema registry. For testing on a local development machine where
all the services are running locally, these could be replaced with __localhost__.
```
export CONFLUENT_HOME=/opt/confluent
export CONFLUENT_BOOTSTRAP_SERVERS=<ip.to.broker>:9092
export CONFLUENT_SCHEMA_REGISTRY_URL=http://<ip.to.schema.registry>:8081
export PATH=$PATH:$CONFLUENT_HOME/bin
```

To apply the changes to your current session:
```
$ source /etc/profile
```

### Update Zookeeper Configuration
Backup the original configuration, and make changes to the configuration.
```
$ sudo cp /opt/confluent/etc/kafka/zookeeper.properties /opt/confluent/etc/kafka/zookeeper.properties.orig \
    --preserve=ownership
$ sudo nano /opt/confluent/etc/kafka/zookeeper.properties
```

Replace existing configuration values with the ones below.
```
# the port at which the clients will connect
clientPort=2181

# the directory for storing in-memory database snapshots
dataDir=/var/lib/zookeeper

# the directory for storing transaction log of updates to the database
# NOTE: in production, this should be on a differernt device from dataDir
dataLogDir=/var/lib/zookeeper

# purge task interval every 12 hours
autopurge.purgeInterval=12
```

### Update Kafka Configuration
Backup the original configuration, and make changes to the configuration.
```
$ sudo cp /opt/confluent/etc/kafka/server.properties /opt/confluent/etc/kafka/server.properties.orig \
    --preserve=ownership
$ sudo nano /opt/confluent/etc/kafka/server.properties
```

Replace existing configuration values with the ones below.
```
# change default (/tmp/kafka-logs) to a more durable location
log.dirs=/var/lib/kafka-logs
```

### Update Kafka Connect Configuration
Backup the original configuration, and make changes to the configuration.
```
$ sudo cp /opt/confluent/etc/schema-registry/connect-avro-distributed.properties /opt/confluent/etc/schema-registry/connect-avro-distributed.properties.orig \
    --preserve=ownership
$ sudo nano /opt/confluent/etc/schema-registry/connect-avro-distributed.properties
```

Replace existing configuration values with the ones below.
```
# specify the location of jdbc connector (using absolute path)
plugin.path=/opt/confluent/share/java
```

### Create Systemd Unit Files To Start Services
Create a unit file for Zookeeper.
```
$ sudo nano /etc/systemd/system/zookeeper.service
```

Copy the following contents to the Zookeeper unit file.
```
[Unit]
Description=unit to start and stop the Zookeeper service
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=kafka
Groupp=nogroup
ExecStart=/opt/confluent/bin/zookeeper-server-start /opt/confluent/etc/kafka/zookeeper.properties
ExecStop=/opt/confluent/bin/zookeeper-server-stop
Restart=on-abnormal
```

Create a unit file for Kafka.
```
$ sudo nano /etc/systemd/system/kafka.service
```

Copy the following contents to the Kafka unit file.
```
[Unit]
Description=unit to start and stop the Kafka service
Requires=network.target remote-fs.target zookeeper.service
After=network.target remote-fs.target zookeeper.service

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=kafka
Groupp=nogroup
ExecStart=/opt/confluent/bin/kafka-server-start /opt/confluent/etc/kafka/server.properties
ExecStop=/opt/confluent/bin/kafka-server-stop
Restart=on-abnormal
```

Create a unit file for Schema Registry.
```
$ sudo nano /etc/systemd/system/schema-registry.service
```

Copy the following contents to the Schema Registry unit file.
```
[Unit]
Description=unit to start and stop the Schema Registry service
Requires=network.target remote-fs.target kafka.service
After=network.target remote-fs.target kafka.service

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=kafka
Groupp=nogroup
ExecStart=/opt/confluent/bin/schema-registry-start /opt/confluent/etc/schema-registry/schema-registry.properties
ExecStop=/opt/confluent/bin/schema-registry-stop
Restart=on-abnormal
```

Create a unit file for Kafka Connect.
```
$ sudo nano /etc/systemd/system/kafka-connect.service
```

Copy the following contents to the Kafka Connect unit file.
```
[Unit]
Description=unit to start and stop the Kafka Connect service
Requires=network.target remote-fs.target schema-registry.service
After=network.target remote-fs.target schema-registry.service

[Install]
WantedBy=multi-user.target

[Service]
Type=simple
User=kafka
Groupp=nogroup
ExecStart=/opt/confluent/bin/connect-distributed /opt/confluent/etc/schema-registry/connect-avro-distributed.properties
ExecStop=kill $(ps -ef | grep '[C]onnectDistributed' | awk '{print $2}')
Restart=on-abnormal
```

### Auto Start Services
Enable the services to automatically start with the system.
```
$ sudo systemctl enable zookeeper.service
$ sudo systemctl enable kafka.service
$ sudo systemctl enable schema-registry.service
$ sudo systemctl enable kafka-connect.service
```

### Manually Start Services
```
$ sudo systemctl start zookeeper.service
$ sudo systemctl start kafka.service
$ sudo systemctl start schema-registry.service
$ sudo systemctl start kafka-connect.service
```

### Manually Service Status
```
$ sudo systemctl status zookeeper.service
$ sudo systemctl status kafka.service
$ sudo systemctl status schema-registry.service
$ sudo systemctl status kafka-connect.service
```

### Verify Installation (Producer/Consumer)
To test Kafka, we will create a Kafka topic (__test__), run a producer to
publish Avro messages to the __test__ topic, and run a consumer to subscribe
to Avro messages from the __test__ topic.
```
Producer -> test topic -> Consumer
```

Open three terminal windows.

In terminal 1, create a Kafka topic (__test__).
```
$ sudo /opt/confluent/bin/kafka-topics --create --topic test \
    --bootstrap-server $CONFLUENT_BOOTSTRAP_SERVERS \
    --replication-factor 1 --partitions 1
```

In terminal 2, start a producer to send messages to the __test__ topic.
```
$ sudo /opt/confluent/bin/kafka-avro-console-producer --topic test \
    --broker-list $CONFLUENT_BOOTSTRAP_SERVERS \
    --property value.schema='{"type":"record","name":"testrecord","fields":[{"name":"message","type":"string"}]}'
```

Once the producer has started, you can manually input messages, one line at a time. Input 
a few test messages:
```
{"message": "this is a testA"}
{"message": "this is a testB"}
{"message": "this is a testC"}
```
In terminal 3, start a consumer to read messages from the __test__ topic.
```
$ sudo /opt/confluent/bin/kafka-avro-console-consumer --topic test \
    --bootstrap-server $CONFLUENT_BOOTSTRAP_SERVERS --from-beginning
```

In the output, you should see the messages sent earlier by the producer from 
terminal 2.

### Verify Installation (Kafka Connect)
To test Kafka Connect, we will create a new database (__kafka_test__) with a
table (__messages__) in MariaDB. Previously, we verified that we can publish an 
Avro message from a producer to the __test__ topic, and consume the Avro 
messages from the __test__ topic. Now, we will test that the Avro messages 
published to the __test__ topic can be exported to a database table 
(__kafka_test.messages__), and that the data records saved to the database table 
(__kafka_test.messages__) can be ingested to a Kafka topic 
(__mariadb-messages__) where it can be consumed by another consumer.
```
Producer -> test topic -> Consumer
                        -> messages table -> mariadb-messages topic -> Consumer2
```

Open three terminal windows.

In terminal 1, Login as __root__.
```
$ sudo mysql
```

From within the shell, create a new database (__kafka_test__) and table 
(__messages__).
```
MariaDB [(none)] > CREATE DATABASE IF NOT EXISTS kafka_test;
MariaDB [(none)] > USE kafka_test;
MariaDB [(none)] > CREATE TABLE IF NOT EXISTS messages (message_id INT(11) NOT NULL AUTO_INCREMENT, message VARCHAR(20) NOT NULL, created_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP, CONSTRAINT messages_pk PRIMARY KEY (message_id));
MariaDB [(none)] > EXIT
``` 

Create a sink connector to export Avro messages from the __test__ topic to the
__kakfa_test.messages__ table. Replace __admin__ and __strong_password__ with
your mysql username and password.
```
$ curl -X POST -H "Content-Type: application/json" --data '{ 
    "name": "messages-sink", "config": {
      "connector.class": "io.confluent.connect.jdbc.JdbcSinkConnector", 
      "key.converter": "io.confluent.connect.avro.AvroConverter", 
      "key.converter.schema.registry.url": "http://localhost:8081", 
      "value.converter": "io.confluent.connect.avro.AvroConverter", 
      "value.converter.schema.registry.url": "http://localhost:8081", 
      "tasks.max": 1, 
      "connection.url": "jdbc:mariadb://localhost:3306/kafka_test", 
      "connection.user": "admin", 
      "connection.password": "strong_password", 
      "topics": "test", 
      "table.name.format" : "messages", 
      "insert.mode": "insert", 
      "pk.mode": "none"}}' \
    http://localhost:8083/connectors | jq
```

Create a source connector to ingest data records from the database table
(__kafka_test.messages__) to the Kafka topic (__mariadb-messages__).
Replace __admin__ and __strong_password__ with your mysql username and 
password.
```
$ curl -X POST -H "Content-Type: application/json" --data '{ 
    "name": "messages-source", "config": {
      "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector", 
      "key.converter": "io.confluent.connect.avro.AvroConverter", 
      "key.converter.schema.registry.url": "http://localhost:8081", 
      "value.converter": "io.confluent.connect.avro.AvroConverter", 
      "value.converter.schema.registry.url": "http://localhost:8081", 
      "tasks.max": 1, 
      "connection.url": "jdbc:mariadb://localhost:3306/kafka_test", 
      "connection.user": "admin", 
      "connection.password": "strong_password", 
      "table.whitelist": "messages", 
      "mode": "timestamp", 
      "timestamp.column.name": "created_on", 
      "topic.prefix": "mariadb-", 
      "poll.interval.ms": 1000}}' \
    http://localhost:8083/connectors | jq
```

In terminal 2, start a consumer to listen to the Kafka topic 
(__mariadb-message__).
```
$ sudo /opt/confluent/bin/kafka-avro-console-consumer --topic mariadb-messages \
    --bootstrap-server $CONFLUENT_BOOTSTRAP_SERVERS --from-beginning
```

In the output, you should see the Avro messages that were previously consumed
from the Kafka topic (__test__).
```
{"message_id":1,"message":"this is a testA","created_on":1595709388000}
{"message_id":2,"message":"this is a testB","created_on":1595709399000}
{"message_id":3,"message":"this is a testC","created_on":1595709407000}
```

Back in terminal one, if you query the contents of the database table 
(__kafka_test.messages__), you should now see that the table has been populated 
with the messages that were originally published to the Kafka topic (__test__).
```
MariaDB [kafka_test]> select * from messages;
+------------+-----------------+---------------------+
| message_id | message         | created_on          |
+------------+-----------------+---------------------+
|          1 | this is a testA | 2020-07-25 20:36:28 |
|          2 | this is a testB | 2020-07-25 20:36:39 |
|          3 | this is a testC | 2020-07-25 20:36:47 |
+------------+-----------------+---------------------+
3 rows in set (0.000 sec)
```

### Manually Stop Services
```
$ sudo systemctl stop kafka-connect.service
$ sudo systemctl stop schema-registry.service
$ sudo systemctl stop kafka.service
$ sudo systemctl stop zookeeper.service
```

### Cleanup Test
```
$ curl -X DELETE http://localhost:8083/connectors/messages-source
$ curl -X DELETE http://localhost:8083/connectors/messages-sink
$ sudo /opt/confluent/bin/kafka-topics --delete --topic mariadb-messages --bootstrap-server $CONFLUENT_BOOTSTRAP_SERVERS
$ sudo /opt/confluent/bin/kafka-topics --delete --topic test --bootstrap-server $CONFLUENT_BOOTSTRAP_SERVERS
$ sudo mysqladmin drop kafka_test
```
### Uninstall
```
$ sudo rm -rf /opt/confluent
$ sudo rm -rf /var/lib/zookeeper
$ sudo rm -rf /var/lib/kafka-logs
$ sudo rm -rf /var/lib/kafka-streams
$ sudo rm -f /etc/profile.d/confluent-vars.sh
$ sudo rm -f /etc/systemd/system/zookeeper.service
$ sudo rm -f /etc/systemd/system/kafka.service
$ sudo rm -f /etc/systemd/system/schema-registry.service
```