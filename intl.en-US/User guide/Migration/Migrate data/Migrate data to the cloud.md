---
keyword: [kafka, mirrormaker, topic, migration]
---

# Migrate data to the cloud

This topic describes how to use MirrorMaker to migrate data in a user-created Kafka cluster to a Message Queue for Apache Kafka instance.

The following operations are completed:

-   [Download MirrorMaker](http://kafka.apache.org/downloads).
-   [Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka](/intl.en-US/User guide/Migration/Migrate topics/Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache
         Kafka.md).

Kafka mirroring can be used to back up data in Kafka clusters. MirrorMaker is the tool to implement this feature. You can use MirrorMaker to mirror the source user-created Kafka cluster to the destination cluster. The destination cluster is a Message Queue for Apache Kafka instance, as shown in the following figure. MirrorMaker uses a built-in consumer to consume messages from the user-created Kafka cluster and then uses a built-in producer to send these messages to the Message Queue for Apache Kafka instance.

![dg_data_migration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2450549951/p98881.png)

Fore more information, see [Apache Kafka MirrorMaker](https://kafka.apache.org/documentation/#basic_ops_mirror_maker).

## Precautions

-   The topic names of the source and destination clusters must be consistent.
-   The numbers of partitions of the source and destination clusters can be different.
-   Data in the same partition may not be distributed to the same partition.
-   By default, messages with the same key are distributed to the same partition.
-   Normal messages may be out of order when the instance fails, whereas partitionally ordered messages remain in order when the instance fails.

## Access from a VPC

1.  Configure the consumer.properties file.

    ```
    ## The endpoint of the user-created Kafka cluster.
    bootstrap.servers=XXX.XXX.XXX.XXX:9092
    
    ## The consumer policy for distributing messages to partitions.
    partition.assignment.strategy=org.apache.kafka.clients.consumer.RoundRobinAssignor
    
    ## The name of the consumer group.
    group.id=test-consumer-group
    ```

2.  Configure the producer.properties file.

    ```
    ## The default endpoint of the Message Queue for Apache Kafka instance, which can be obtained in the Message Queue for Apache Kafka console.
    bootstrap.servers=XXX.XXX.XXX.XXX:9092
    
    ## The data compression method.
    compression.type=none                                
    ```

3.  Run the following command to start the migration process:

    ```
    sh bin/kafka-mirror-maker.sh --consumer.config config/consumer.properties --producer.config config/producer.properties --whitelist topicName
    ```


## Access from the Internet

1.  Download [kafka.client.truststore.jks](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-mirror-maker-demo/kafka.client.truststore.jks?raw=true).

2.  Configure the kafka\_client\_jaas.conf file.

    ```
    KafkaClient {
       org.apache.kafka.common.security.plain.PlainLoginModule required
       username="your username"
       password="your password";
    };
    ```

3.  Configure the consumer.properties file.

    ```
    ## The endpoint of the user-created Kafka cluster.
    bootstrap.servers=XXX.XXX.XXX.XXX:9092
    
    ## The consumer policy for distributing messages to partitions.
    partition.assignment.strategy=org.apache.kafka.clients.consumer.RoundRobinAssignor
    
    ## The name of the consumer group.
    group.id=test-consumer-group
    ```

4.  Configure the producer.properties file.

    ```
    ## The SSL endpoint of the Message Queue for Apache Kafka instance, which can be obtained in the Message Queue for Apache Kafka console.
    bootstrap.servers=XXX.XXX.XXX.XXX:9093
    
    ## The data compression method.
    compression.type=none
    
    ## The truststore (Use the file downloaded in Step 1).
    ssl.truststore.location=kafka.client.truststore.jks
    ssl.truststore.password=KafkaOnsClient
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    
    ## If you use Message Queue for Apache Kafka 2.x with Simple Authentication and Security Layer (SASL) authentication, you must configure the following parameter. This parameter is not required for Message Queue for Apache Kafka earlier than v2.x.
    ssl.endpoint.identification.algorithm=
    ```

5.  Configure the java.security.auth.login.config file.

    ```
    export KAFKA_OPTS="-Djava.security.auth.login.config=kafka_client_jaas.conf"                              
    ```

6.  Run the following command to start the migration process:

    ```
    sh bin/kafka-mirror-maker.sh --consumer.config config/consumer.properties --producer.config config/producer.properties --whitelist topicName
    ```


## Verify the result

You can check whether MirrorMaker runs by using one of the following methods:

-   Run `kafka-consumer-groups.sh` to view the consumption progress of the user-created cluster.

    `bin/kafka-consumer-groups.sh --new-consumer --describe --bootstrap-server endpoint of the user-created cluster --group test-consumer-group`

-   Send messages to the user-created cluster. In the Message Queue for Apache Kafka console, check the partition status of the topic, and check whether the total number of messages in the current broker is correct. You can view the specific content of a message in the Message Queue for Apache Kafka console. For more information, see [Query messages](/intl.en-US/User guide/Query messages.md).

