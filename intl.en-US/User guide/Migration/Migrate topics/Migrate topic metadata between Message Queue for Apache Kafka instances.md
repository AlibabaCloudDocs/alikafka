---
keyword: [topic, metadata, migration, Message Queue for Apache Kafka]
---

# Migrate topic metadata between Message Queue for Apache Kafka instances

This topic describes how to use a metadata migration tool to migrate topic metadata from a Message Queue for Apache Kafka instance to another Message Queue for Apache Kafka instance. The metadata of a topic is the basic information of the topic instead of the information stored in the topic.

Before you migrate topic metadata between Message Queue for Apache Kafka instances, make sure that you have completed the following steps:

-   [Download Java Development Kit \(JDK\) 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [Download the migration tool Java Archive \(JAR\) file](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**Note:**

-   After the migration, the corresponding topic in the source Message Queue for Apache Kafka is not deleted. Instead, a new topic with the same configuration is created in the destination Message Queue for Apache Kafka instance.
-   Only the topic configuration is migrated, whereas the messages in the topic are not migrated.

1.  Start the command-line tool.

2.  Run the cd command to switch to the directory where the migration tool is located.

3.  Run the following command to confirm the topic metadata to be migrated:

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromAliyun --sourceAk <yoursourceAccessKeyId> --sourceSk <yoursourceAccessKeySecret> --sourceRegionId <yoursourceRegionId> --sourceInstanceId <yoursourceInstanceId> --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId>`

    |Parameter|Description|
    |---------|-----------|
    |sourceAk|The AccessKey ID of the Alibaba Cloud account to which the source Message Queue for Apache Kafka instance belongs.|
    |sourceSk|The AccessKey secret of the Alibaba Cloud account to which the source Message Queue for Apache Kafka instance belongs.|
    |sourceRegionId|The ID of the region where the source Message Queue for Apache Kafka instance is located.|
    |sourceInstanceId|The ID of the source Message Queue for Apache Kafka instance.|
    |destAk|The AccessKey ID of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destSk|The AccessKey secret of the Alibaba Cloud account to which the destination Message Queue for Apache Kafka instance belongs.|
    |destRegionId|The ID of the region where the destination Message Queue for Apache Kafka instance is located.|
    |destInstanceId|The ID of the destination Message Queue for Apache Kafka instance.|

    The following code provides an example of the output to be confirmed:

    ```
    15:13:12 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"total":4,"code":200,"requestId":"1CBAB340-2146-43A3-8470-84D77DB8B43E","success":true,"pageSize":10000,"currentPage":1,"message":"operation success.","topicList":[{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558314000,"regionId":"cn-hangzhou","statusName":"Running","topic":"agdagasdg","remark":"agdadgdasg","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558294000,"regionId":"cn-hangzhou","statusName":"Running","topic":"135215","remark":"1315215","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558214000,"regionId":"cn-hangzhou","statusName":"Running","topic":"1332","remark":"13414","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558141000,"regionId":"cn-hangzhou","statusName":"Running","topic":"aete","remark":"est","partitionNum":12,"compactTopic":false,"status":0,"tags":[]}]}
    15:13:12 INFO - Will create topic:agdagasdg, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:135215, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:1332, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:aete, isCompactTopic:false, partition number:12
    ```

4.  Run the following command to commit the topic metadata to be migrated:

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromAliyun --sourceAk <yoursourceAccessKeyId> --sourceSk <yoursourceAccessKeySecret> --sourceRegionId <yoursourceRegionId> --sourceInstanceId <yoursourceInstanceId> --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId> --commit`

    |Parameter|Description|
    |---------|-----------|
    |commit|Commits the topic metadata to be migrated.|

    The following code provides an example of the output after the preceding commit:

    ```
    16:38:30 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"A0CA4D70-46D4-45CF-B9E0-B117610A26DB","success":true,"message":"operation success"}
    16:38:30 INFO - TopicCreate success, topic=agdagasdg, partition number=12, isCompactTopic=false
    16:38:36 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"05E88C75-64B6-4C87-B962-A63D906FD993","success":true,"message":"operation success"}
    16:38:36 INFO - TopicCreate success, topic=135215, partition number=12, isCompactTopic=false
    16:38:42 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"9D54F6DB-6FA0-4F6D-B19A-09109F70BDDA","success":true,"message":"operation success"}
    16:38:42 INFO - TopicCreate success, topic=1332, partition number=12, isCompactTopic=false
    16:38:49 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"6C265013-D15E-49AE-BE55-BF7657ADA1B7","success":true,"message":"operation success"}
    16:38:49 INFO - TopicCreate success, topic=aete, partition number=12, isCompactTopic=false
    ```

5.  Check whether the topic metadata is migrated.

    1.  Log on to the [Message Queue for Apache Kafkaconsole](https://kafka.console.aliyun.com/).

    2.  In the top navigation bar, select the region where the destination instance is located.

    3.  In the left-side navigation pane, click **Topics**.

    4.  On the **Topics** page, click the destination instance.

        The migrated topic appears on the **Topics** page.

        ![Migration](../images/p88767.png)


