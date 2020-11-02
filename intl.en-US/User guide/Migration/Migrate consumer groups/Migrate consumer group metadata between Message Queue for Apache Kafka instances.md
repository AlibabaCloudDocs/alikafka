---
keyword: [consumer group, metadata, migration, Message Queue for Apache Kafka]
---

# Migrate consumer group metadata between Message Queue for Apache Kafka instances

This topic describes how to use a metadata migration tool to migrate consumer group metadata from one Message Queue for Apache Kafka instance to another Message Queue for Apache Kafka instance.

Before you migrate consumer group metadata between Message Queue for Apache Kafka instances, make sure that you have completed the following steps:

-   [Download Java Development Kit \(JDK\) 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [Download the migration tool Java Archive \(JAR\) file](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**Note:**

-   After the migration, the corresponding consumer group in the source Message Queue for Apache Kafka instance is not deleted. Instead, a new consumer group with the same configuration is created in the destination Message Queue for Apache Kafka instance.
-   Only the consumer group configuration is migrated. Topics and consumer offsets in the consumer group are not migrated.

1.  Start the command-line tool.

2.  Run the cd command to switch to the directory where the migration tool is located.

3.  Run the following command to confirm the consumer group metadata to be migrated:

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
    10:54:26 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"9793DADB-55A5-4D4E-9E9C-D4DA8B35370C","success":true,"consumerList":[{"instanceId":"alikafka_post-cn-0pp1h0uv6***","regionId":"cn-hangzhou","consumerId":"Demo","tags":[{"value":"","key":"migration"}]}],"message":"operation success."}
    10:54:26 INFO - Will create consumer groups:[Demo]
    ```

4.  Run the following command to commit the consumer group metadata to be migrated:

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromAliyun --sourceAk LTAI4FwQ5aK1mFYCspJ1h*** --sourceSk wvDxjjRQ1tHPiL0oj7Y2Z7WDNkS*** --sourceRegionId cn-hangzhou --sourceInstanceId alikafka_post-cn-0pp1h0uv6*** --destAk LTAI4FwQ5aK1mFYCspJ1h*** --destSk wvDxjjRQ1tHPiL0oj7Y2Z7*** --destRegionId cn-hangzhou --destInstanceId alikafka_pre-cn-v0h1cng00*** --commit`

    |Parameter|Description|
    |---------|-----------|
    |commit|Commits the consumer group metadata to be migrated.|

    The following code provides an example of the output after the preceding commit:

    ```
    10:54:40 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"49E53B79-3C2C-4BCF-8BC8-07B0BB14B52A","success":true,"consumerList":[{"instanceId":"alikafka_post-cn-0pp1h0uv6***","regionId":"cn-hangzhou","consumerId":"Demo","tags":[{"value":"","key":"migration"}]}],"message":"operation success."}
    10:54:41 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"5AEEFB13-2A6B-4265-97CB-902CFA483339","success":true,"message":"operation success"}
    10:54:41 INFO - ConsumerCreate success, consumer group=Demo
    ```

5.  Check whether the consumer group metadata is migrated.

    1.  Log on to the [Message Queue for Apache Kafkaconsole](https://kafka.console.aliyun.com/).

    2.  In the top navigation bar, select the region where the destination instance is located.

    3.  In the left-side navigation pane, click **Consumer Groups**.

    4.  On the **Consumer Groups** page, click the destination instance.

        The created consumer group appears on the **Consumer Group** page.

        ![Consumer group migration](../images/p88862.png)


