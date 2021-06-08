# KAFKA SETUP

Install and extract kafka  

Start zookeeper:

    $ cd <dir>
    $ sudo bin/zookeeper-server-start.sh config/zookeeper.properties

Start server:

    $ bin/kafka-server-start.sh config/server.properties

View topics:
    
    $ sudo bin/kafka-topics.sh --list --bootstrap-server <host>:<port>

Create topic:
    
    $ sudo bin/kafka-topics.sh --create --topic test-123 --bootstrap-server <host>:<port> --replication-factor 1 --partitions 1

Describe topic:

    $ sudo bin/kafka-topics.sh --describe --topic test-123 --bootstrap-server <host>:<port>

Produce to topic:

    $ sudo bin/kafka-console-producer.sh --topic test-123 --bootstrap-server <host>:<port>

Consume from topic:

    $ sudo sudo bin/kafka-console-consumer.sh --topic test-123 --from-beginning --bootstrap-server <host>:<port>
