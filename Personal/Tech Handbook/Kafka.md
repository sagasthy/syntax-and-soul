# Kafka

Kafka Architecture Overview
![[Pasted image 20251027112038.png]]

Kafka Notes
## Commands

### Zookeeper Start

./zookeeper-server-start.sh ../config/zookeeper.properties

### Kafka Start

./kafka-server-start.sh ../config/server.properties

### Create a kafka topic

kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor

### List all topics

kafka-topics --zookeeper 127.0.0.1:2181 --list

### Details of a topic

kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --describe

### Delete a topic

kafka-topics --zookeeper 127.0.0.1:2181 --topic second_topic --delete

### Produce a message from CLI

[kafka-console-producer.sh](http://kafka-console-producer.sh/) --broker-list 127.0.0.1:9092 --topic first_topic

→ We can also specify properties using the below command

[kafka-console-producer.sh](http://kafka-console-producer.sh/) --broker-list 127.0.0.1:9092 --topic first_topic —producer-property acks=all

**Note:** if a topic doesn't already exist while running the above command, producing will create the topic itself (defaulting to 1 partition and a replication factor of 1 which is not recommended), this is why it's always good to create a topic before hand. Also we can change the default number of partitions in config/server.properties file.