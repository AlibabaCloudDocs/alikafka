---
keyword: [consumer group, metadata, migration, Message Queue for Apache Kafka]
---

# Migrate consumer group metadata from a user-created Kafka cluster to Message Queue for Apache Kafka

This topic describes how to use a metadata migration tool to migrate consumer group metadata from a user-created Kafka cluster to a Message Queue for Apache Kafka instance.

Before you migrate consumer group metadata from a user-created Kafka cluster to Message Queue for Apache Kafka, make sure that you have completed the following steps:

-   [Download Java Development Kit \(JDK\) 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [Download the migration tool Java Archive \(JAR\) file](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**Note:**

-   After the migration, the corresponding consumer group in the source Kafka cluster is not deleted. Instead, a new consumer group with the same configuration is created in the destination Message Queue for Apache Kafka instance.
-   Only the consumer group configuration is migrated. Topics and consumer offsets are not migrated.

1.  Start the command-line tool.

2.  Run the cd command to switch to the directory where the migration tool is located.

3.  Create a configuration file named kafka.properties.

    The kafka.properties file is used to create a Kafka consumer to retrieve consumer offset information from the user-created Kafka cluster. The configuration file contains the following content:

    ```
    ## The endpoint.
    bootstrap.servers=localhost:9092
    
    ## The consumer group, which contains no consumer offset information so that consumption starts from the first message.
    group.id=XXX
    
    ## You can skip the following configuration if security configuration is unavailable.
    
    ## The simple authentication and security layer (SASL)-based authentication.
    #sasl.mechanism=PLAIN
    
    ## The access protocol.
    #security.protocol=SASL_SSL
    
    ## The path of the Secure Sockets Layer (SSL) root certificate.
    #ssl.truststore.location=/Users/***/Documents/code/aliware-kafka-demos/main/resources/kafka.client.truststore.jks
    
    ## The SSL password.
    #ssl.truststore.password=***
    
    ## The SASL path.
    #java.security.auth.login.config=/Users/***/kafka-java-demo/vpc-ssl/src/main/resources/kafka_client_jaas.conf
    ```

4.  Run the following command to confirm the consumer group metadata to be migrated:

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromTopic --propertiesPath /usr/local/kafka_2.12-2.4.0/config/kafka.properties --destAk <yourAccessKeyId> --destSk <yourAccessKeySecret> --destRegionId <yourRegionId> --destInstanceId <yourInstanceId>`

    |Parameter|Description|
    |---------|-----------|
    |propertiesPath|The path of the kafka.properties configuration file.|
    |destAk|The AccessKey ID of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destSk|The AccessKey secret of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destRegionId|The ID of the region where the destination Message Queue for Apache Kafka instance is located.|
    |destInstanceId|The ID of the destination Message Queue for Apache Kafka instance.|

    The following code provides an example of the output to be confirmed:

    ```
    15:29:45 INFO - Will create consumer groups:[XXX, test-consumer-group]
    ```

5.  Run the following command to commit the consumer group metadata to be migrated:

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromTopic --propertiesPath /usr/local/kafka_2.12-2.4.0/config/kafka.properties --destAk LTAI4FwQ5aK1mFYCspJ1h*** --destSk wvDxjjRQ1tHPiL0oj7Y2Z7WDNkS*** --destRegionId cn-hangzhou --destInstanceId alikafka_pre-cn-v0h1cng00*** --commit`

    |Parameter|Description|
    |---------|-----------|
    |commit|Commits the consumer group metadata to be migrated.|

    The following code provides an example of the output after the preceding commit:

    ```
    15:35:51 INFO - cmd=ConsumerGroupMigrationFromTopic, request=null, response={"code":200,"requestId":"C9797848-FD4C-411F-966D-0D4AB5D12F55","success":true,"message":"operation success"}
    15:35:51 INFO - ConsumerCreate success, consumer group=XXX
    15:35:57 INFO - cmd=ConsumerGroupMigrationFromTopic, request=null, response={"code":200,"requestId":"3BCFDBF2-3CD9-4D48-92C3-385C8DBB9709","success":true,"message":"operation success"}
    15:35:57 INFO - ConsumerCreate success, consumer group=test-consumer-group
    ```

6.  Check whether the consumer group metadata is migrated.

    1.  Log on to the [Message Queue for Apache Kafkaconsole](https://kafka.console.aliyun.com/).

    2.  In the top navigation bar, select the region where the destination instance is located.

    3.  In the left-side navigation pane, click **Consumer Groups**.

    4.  On the **Consumer Groups** page, click the destination instance.

        The migrated consumer group appears on the **Consumer Group** page.


