# Set up Kafka in Ubuntu

```
$ sudo apt-get update
$ sudo apt-get install default-jre

$ sudo apt-get install zookeeperd

# download kafka .tgz file in Downloads directory
$ curl "https://www.apache.org/dyn/closer.cgi?path=/kafka/3.2.0/kafka-3.2.0-src.tgz" -o ~/Downloads/kafka.tgz

$ mkdir ~/kafka && cd ~/kafka

# extract kafka .tgz file
$ tar -xvzf ~/Downloads/kafka.tgz --strip 1

$ nano ~/kafka/config/server.properties

# add the following:
  delete.topic.enable = true

$ sudo nano /etc/systemd/system/zookeeper.service

# add the following:
  [Unit]
  Requires=network.target remote-fs.target
  After=network.target remote-fs.target

  [Service]
  Type=simple
  User=hrittik
  ExecStart=/home/hrittik/kafka/bin/zookeeper-server-start.sh /home/hrittik/kafka/config/zookeeper.properties
  ExecStop=/home/hrittik/kafka/bin/zookeeper-server-stop.sh
  Restart=on-abnormal

  [Install]
  WantedBy=multi-user.target

# find jre path
$ java -XshowSettings:properties -version 2>&1 > /dev/null | grep 'java.home'
  
$ sudo nano /etc/systemd/system/kafka.service

# add the following
  [Unit]
  Requires=zookeeper.service
  After=zookeeper.service

  [Service]
  Type=simple
  User=hrittik
  Environment="JAVA_HOME=<jre_path>"
  ExecStart=/home/hrittik/kafka/bin/kafka-server-start.sh /home/hrittik/kafka/config/server.properties
  ExecStop=/home/hrittik/kafka/bin/kafka-server-stop.sh
  Restart=on-abnormal

  [Install]
  WantedBy=multi-user.target
 
$ sudo systemctl start kafka
$ sudo systemctl status kafka 
```
