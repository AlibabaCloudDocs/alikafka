# Step 3: Create resources

Before using Message Queue for Apache Kafka to send and subscribe to messages, you must create resources in the Message Queue for Apache Kafka console. Otherwise, you cannot pass authentication or use the management and maintenance functions. Resources here refer to topics and consumer groups.

## Step 1: Create a topic

Topic is the first-level identifier for classifying messages in Message Queue for Apache Kafka. For example, you can create a topic named Topic\_Trade for transaction messages. The first step to use Message Queue for Apache Kafka is to create a topic for your application.

Follow these steps to create a topic:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com/). In the top navigation bar, select a region, such as **China \(Hangzhou\)**.
2.  In the left-side navigation pane, click **Topics**.
3.  On the Topics page, click **Create Topic**.
4.  The required topic information is related to the payment method and instance version:
    -   For Standard Edition:

        In the Create Topic dialog box, enter the topic name, select an instance, enter the description, the number of partitions, and tags, and then click **Create**.

    -   For Professional Edition:

        In the Create Topic dialog box, enter the topic name, select an instance, and enter the description, the number of partitions, and tags. Click **Advanced Settings**, select **Storage Engine** and **Message Type**, and then click **Create**.

        The advanced settings are described as follows:

        |Parameter|Description|
        |---------|-----------|
        |Storage Engine|Currently, Message Queue for Apache Kafka provides two storage engines:

         -   Cloud Storage: It accesses Alibaba Cloud disks at the underlying layer and stores three replicas in distributed mode. It features low latency, high performance, persistence, and high reliability. If you need 900 GB of storage space, you have to buy 2700 GB of storage space due to the three-replica mechanism.
        -   Local Storage: Based on the In-Sync Replicas \(ISR\) algorithm of Apache Kafka, three replicas are stored in distributed mode and `min.insync.replicas is set to 2`. If you need 900 GB of storage space, you have to buy 2700 GB of storage space due to the three-replica mechanism.

**Note:** Currently, only subscription Message Queue for Apache Kafka instances of the Professional Edition whose Apache Kafka version is 2.2.0 support the local storage mode. For more information about how to upgrade your instance edition, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

 For more information about comparison of two storage engines, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md). |
        |cleanup.policy|You need to configure a log cleanup policy \(cleanup.policy\) only in Local Storage mode. Currently, Message Queue for Apache Kafka provides two log cleanup policies:

         -   delete: It is the default message cleanup policy. If the remaining disk space is sufficient, messages are retained for the maximum retention period. If the remaining disk space is insufficient \(the disk usage exceeds 85%\), historical messages are deleted to ensure service availability.
        -   compact: The [Apache Kafka log compaction policy](https://kafka.apache.org/documentation/?spm=a2c4g.11186623.2.15.1cde7bc3c8pZkD#compaction) is used. Based on the log compaction policy, if the keys of the messages are the same, messages that have the latest key values are retained. This policy is applicable to scenarios where the system is recovered from a system failure or the cache is reloaded after system restart. For example, when using Kafka Connect or Confluent Schema Registry, you need to store the system status information or configuration information in a log compacted topic.

**Note:** Log compacted topics are generally used only in some ecosystem components, such as Kafka Connect or Confluent Schema Registry. Do not set this attribute for the topic for sending and subscribing to messages in other components. For more information, see the [demo documentation](https://github.com/AliwareMQ/aliware-kafka-demos?spm=a2c4g.11186623.2.16.1cde7bc3c8pZkD) of each ecosystem component. |
        |Message Type|Currently, Message Queue for Apache Kafka supports two types of messages:

         -   Common message: By default, messages of the same key are stored in the same partition in the order they are sent. When an instance in the cluster fails, the messages may be out of order.
        -   Partitionally ordered message: By default, messages of the same key are stored in the same partition in the order they are sent. When an instance in the cluster fails, the messages are stilled stored in the partition in the order they are sent.. However, some messages in the partition cannot be sent until the partition is restored. |


After the topic is created, it appears in the list on the Topics page.

**Note:**

-   You must create a topic in the region where the application is located. That is, the region where the Elastic Compute Service \(ECS\) instance is deployed.
-   A topic cannot be used cross regions. For example, if a topic is created in China \(Beijing\), the message producer and consumer must run on ECS instances in China \(Beijing\).
-   For more information about regions, see [Regions and zones](/intl.en-US/Product Introduction/Regions and zones.md) in the ECS documentation.

## Step 2: Create a consumer group

After creating a topic, create a consumer group as follows:

1.  In the left-side navigation pane in the Message Queue for Apache Kafka console, click **Consumer Groups**.
2.  On the Consumer Groups page, click **Create Consumer Group**.
3.  In the Create Consumer Group dialog box, set the consumer group name, select the instance and tags for the consumer group, and then click **Create**.

After the consumer group is created, it appears in the list on the Consumer Groups page.

**Note:** The relationship between consumer groups and topics is N:N. One consumer group can subscribe to multiple topics, and one topic can be subscribed to by multiple consumer groups.

