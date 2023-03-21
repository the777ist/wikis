# KAFKA SETUP

```bash
Kafka:
# install and extract kafka

$ cd kafka_2.13-2.7.0/
$ sudo bin/zookeeper-server-start.sh config/zookeeper.properties

# new window
$ bin/kafka-server-start.sh config/server.properties

# new window
# view topics
$ sudo bin/kafka-topics.sh --list --bootstrap-server 10.0.20.51:30747

# create topic
$ sudo bin/kafka-topics.sh --create --topic test-123 --bootstrap-server 10.0.20.51:30747 --replication-factor 1 --partitions 1

# describe topic
$ sudo bin/kafka-topics.sh --describe --topic test-123 --bootstrap-server 10.0.20.51:30747

# produce to topic
$ sudo bin/kafka-console-producer.sh --topic test-123 --bootstrap-server 10.0.20.51:30747

# consume from topic
$ sudo sudo bin/kafka-console-consumer.sh --topic test-123 --from-beginning --bootstrap-server 10.0.20.51:30747
```