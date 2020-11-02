---
keyword: [topic, metadata, migration, Message Queue for Apache Kafka]
---

# Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka

This topic describes how to use a metadata migration tool to migrate topic metadata from a user-created Kafka cluster to a Message Queue for Apache Kafka instance. The metadata of a topic is the basic information of the topic instead of the information stored in the topic.

Before you migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka, make sure that you have completed the following steps:

-   [Download Java Development Kit \(JDK\) 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [Download the migration tool Java Archive \(JAR\) file](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**Note:**

-   After the migration, the corresponding topic metadata in the source user-created Kafka cluster is not deleted. Instead, a new topic with the same configuration is created in the destination Message Queue for Apache Kafka instance.
-   Only the topic configuration is migrated, whereas the messages in the topic are not migrated.

1.  Start the command-line tool.

2.  Run the cd command to switch to the directory where the migration tool is located.

3.  Run the following command to confirm the topic metadata to be migrated:

    `java -jar kafka-migration.jar TopicMigrationFromZk --sourceZkConnect 192.168.XX.XX --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId>`

    |Parameter|Description|
    |---------|-----------|
    |sourceZkConnect|The IP address of the source user-created ZooKeeper cluster.|
    |destAk|The AccessKey ID of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destSk|The AccessKey secret of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destRegionId|The ID of the region where the destination Message Queue for Apache Kafka instance is located.|
    |destInstanceId|The ID of the destination Message Queue for Apache Kafka instance.|

    The following code provides an example of the output to be confirmed:

    ```
    13:40:08 INFO - Begin to migrate topics:[test]
    13:40:08 INFO - Total topic number:1
    13:40:08 INFO - Will create topic:test, isCompactTopic:false, partition number:1
    ```

4.  Run the following command to commit the topic metadata to be migrated:

    `java -jar kafka-migration.jar TopicMigrationFromZk --sourceZkConnect 192.168.XX.XX --destAk <yourAccessKeyId> --destSk <yourAccessKeySecret> --destRegionId <yourRegionID> --destInstanceId <yourInstanceId> --commit`

    |Parameter|Description|
    |---------|-----------|
    |commit|Commits the topic metadata to be migrated.|

    The following code provides an example of the output after the preceding commit:

    ```
    13:51:12 INFO - Begin to migrate topics:[test]
    13:51:12 INFO - Total topic number:1
    13:51:13 INFO - cmd=TopicMigrationFromZk, request=null, response={"code":200,"requestId":"7F76C7D7-AAB5-4E29-B49B-CD6F1E0F508B","success":true,"message":"operation success"}
    13:51:13 INFO - TopicCreate success, topic=test, partition number=1, isCompactTopic=false
    ```

5.  Check whether the topic metadata is migrated.

    1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

    2.  In the top navigation bar, select the region where the destination instance is located.

    3.  In the left-side navigation pane, click **Topics**.

    4.  On the **Topics** page, click the destination instance.

        The migrated topic appears on the **Topics** page.


