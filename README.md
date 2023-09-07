Setup Kafka Cluster with 3 instance Zookeeper

# Zookeeper Configuration:
We are going to create 3 zookeeper instance in each server.

## Creating the Zookeeper properties files 

Edit for all Zookeeper instance in path file /home/kafka/config

    #server1
    mv zookeeper.properties zookeeper1.properties
    
    #server2
    mv zookeeper1.properties zookeeper2.properties
    
    #server3
    mv zookeeper1.properties zookeeper3.properties

## Create the data directory for all 3 Zookeeper instance to store the data

    #server1
    mkdir -p /home/devarli/kafka/data/zookeeper1
    
    #server2
    mkdir -p /home/devarli/kafka/data/zookeeper1
    
    #server3
    mkdir -p /home/devarli/kafka/data/zookeeper1

## Creating the unique id for each zookeeper instance

    #server1
    echo 1 > /home/devarli/kafka/data/zookeeper1/myid
    
    #server2
    echo 2 > /home/devarli/kafka/data/zookeeper2/myid
    
    #server3
    echo 3 > /home/devarli/kafka/data/zookeeper3/myid

## Edit config file for 3 instance zookeeper.properties

* __Zookeeper-Instance-1:__ in file path /kafka/config

        nano zookeeper1.properties 

Add the below configuration scripts:

    dataDir=/home/devarli/kafka/data/zookeeper1
    clientPort=2181
    #a non-production config
    tickTime=2000
    initLimit=5
    syncLimit=2
    server.1=ipserver1:2666:3666
    server.2=ipserver2:2667:3667
    server.3=ipserver3:2668:3668
    maxClientCnxns=0

* __Zookeeper-Instance-2:__ in file path /kafka/config

        nano zookeeper2.properties 

Add the below configuration scripts:

    dataDir=/home/devarli/kafka/data/zookeeper2
    clientPort=2182
    #a non-production config
    tickTime=2000
    initLimit=5
    syncLimit=2
    server.1=ipserver1:2666:3666
    server.2=ipserver2:2667:3667
    server.3=ipserver3:2668:3668
    maxClientCnxns=0

* __Zookeeper-Instance-3:__ in file path kafka/config/

        nano zookeeper3.properties 

Add the below configuration scripts:

    dataDir=/home/devarli/kafka/data/zookeeper3
    clientPort=2183
    #a non-production config
    tickTime=2000
    initLimit=5
    syncLimit=2
    server.1=ipserver1:2666:3666
    server.2=ipserver2:2667:3667
    server.3=ipserver3:2668:3668
    maxClientCnxns=0


## Create zookeeper systemd service IN EACH SERVER

__Server-1__

    sudo nano /etc/systemd/system/zookeeper.service

Append this:

    [Unit]
    Requires=network.target remote-fs.target
    After=network.target remote-fs.target
    
    [Service]
    Type=simple
    User=devarli
    ExecStart=/home/devarli/kafka/bin/zookeeper-server-start.sh /home/devarli/kafka/config/zookeeper1.properties
    ExecStop=/home/devarli/kafka/bin/zookeeper-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target

## After all has been setup restart the server

    sudo reboot now


# Kafka Broker Configuration:

we are going to create 3 Kafka broker Instance in each server.

## Creating the Server properties files

Edit for all instance server.properties in path file /home/kafka/config

    #server1
    mv server.properties server1.properties
    
    #server2
    mv server.properties server2.properties
    
    #server3
    mv server.properties server3.properties

## Edit config file for 3 instance server.properties

* __Kafka Broker Instance 1:__ in file path /home/kafka/config

        nano server1.properties
   
Add the below configuration scripts:

    broker.id=0
    log.dirs=/home/devarli/kafka/logs1
    port=9093
    advertised.host.name = ipserver1
    zookeeper.connect=ipserver1:2181,ipserver2:2182,ipserver3:2183
    delete.topic.enable = true

Create the log directories in file path /home/kafka
        
    mkdir logs1

* __Kafka Broker Instance 2:__ in file path /kafka/config

        nano server2.properties 

Add the below configuration scripts:

    broker.id=1
    log.dirs=/home/devarli/kafka/logs2
    port=9094
    advertised.host.name = ipserver2
    zookeeper.connect=ipserver1:2181,ipserver2:2182,ipserver3:2183
    delete.topic.enable = true

Create the log directories in file path /home/kafka

    mkdir logs2
    
* __Kafka Broker Instance 3:__ in file path /kafka/config

        nano server3.properties 

Add the below configuration scripts:

    broker.id=2
    log.dirs=/home/devarli/kafka/logs3
    port=9095
    advertised.host.name = ipserver3
    zookeeper.connect=ipserver1:2181,ipserver2:2182,ipserver3:2183

Create the log directories in file path /home/kafka
        
    mkdir logs3

## Create kafka run as a services, FOR IN EACH SERVER

__Server-1__

    sudo nano /etc/systemd/system/kafka.service

Append this:

    [Unit]
    Requires=zookeeper.service
    After=zookeeper.service
    
    [Service]
    Type=simple
    User=devarli
    ExecStart=/bin/sh -c '/home/devarli/kafka/bin/kafka-server-start.sh /home/devarli/kafka/config/server1.properties > /home/devarli/kafka/logs1/kafka.log 2>&1'
    ExecStop=/home/devarli/kafka/bin/kafka-server-stop.sh
    Restart=on-abnormal
    
    [Install]
    WantedBy=multi-user.target

## After all has been setup restart the server

    sudo reboot now

## Reload systemd configuration

    sudo systemctl daemon-reload

## Start zookeeper in each server

In path file /kafka/bin

    systemctl start zookeeper.service

![image](https://github.com/arliputraa/kafka-and-zookeeper-multinode-cluster-setup/assets/110078907/eb2d9129-ec42-4618-9aa3-0b9bd9d25a6a)

Check status zookeeper

    systemctl status zookeeper.service

![image](https://github.com/arliputraa/kafka-and-zookeeper-multinode-cluster-setup/assets/110078907/5dfe9d83-2a20-41b7-95c1-d7d2733239cb)

## Start kafka in each server

In path file /kafka/bin

    systemctl start kafka

![image](https://github.com/arliputraa/kafka-and-zookeeper-multinode-cluster-setup/assets/110078907/a6f7991c-0e7b-4bb3-90b4-600d7ec573f7)

## Check status kafka

    systemctl status kafka

![image](https://github.com/arliputraa/kafka-and-zookeeper-multinode-cluster-setup/assets/110078907/8d649833-c6f7-4926-bae1-286f28404c8b)








