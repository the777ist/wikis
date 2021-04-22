# KAFKA SETUP

Install and extract kafka  

Start zookeeper:

    $ cd <dir>
    $ sudo bin/zookeeper-server-start.sh config/zookeeper.properties

Start server:

    $ bin/kafka-server-start.sh config/server.properties

View topics:
    
    $ sudo bin/kafka-topics.sh --list --bootstrap-server 10.0.20.51:30747

Create topic:
    
    $ sudo bin/kafka-topics.sh --create --topic test-123 --bootstrap-server 10.0.20.51:30747 --replication-factor 1 --partitions 1

Describe topic:

    $ sudo bin/kafka-topics.sh --describe --topic test-123 --bootstrap-server 10.0.20.51:30747

Produce to topic:

    $ sudo bin/kafka-console-producer.sh --topic test-123 --bootstrap-server 10.0.20.51:30747

Consume from topic:

    $ sudo sudo bin/kafka-console-consumer.sh --topic test-123 --from-beginning --bootstrap-server 10.0.20.51:30747
