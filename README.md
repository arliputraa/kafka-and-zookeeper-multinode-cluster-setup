Setup Kafka Cluster and 3 instance Zookeeper

# Kafka Broker Configuration:
we are going to create 3 Kafka broker Instance in each server.




# Zookeeper Configuration:
We are going to create 3 zookeeper instance in each server.

## Creating the Zookeeper properties files 

Edited for all 3 Zookeeper instance to file home/kafka/config/zookeeper.properties

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

__Zookeeper-Instance-1:__ in file path kafka/config/

    nano zookeeper1.properties 

(add the below configuration scripts)

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

__Zookeeper-Instance-2:__ in file path kafka/config/

    nano zookeeper2.properties 

(add the below configuration scripts)

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

__Zookeeper-Instance-3:__ in file path kafka/config/

    nano zookeeper3.properties 

(add the below configuration scripts)

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

__#Server-1__

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

## Start zookeeper

In path file /kafka/bin

    systemctl start zookeeper.service

Check status zookeeper

    systemctl status zookeeper.service





