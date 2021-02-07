---
keyword: [apache kafka, kafka, topic, consumergroup]
---

# Step 3: Create resources

Before you use Message Queue for Apache Kafka to send and subscribe to messages, you must create resources in the Message Queue for Apache Kafka console. Otherwise, you cannot pass authentication or use the management and maintenance features. Resources here are topics and consumer groups.

A Message Queue for Apache Kafka instance is purchased and deployed based on the network type.

-   [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md)
-   [Access from the Internet and VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Access from the Internet and VPC.md)

After you deploy the Message Queue for Apache Kafka instance, you must create topics and consumer groups.

-   A topic is the first-level identifier for classifying messages in Message Queue for Apache Kafka. For example, you can create a topic named Topic\_Trade for transactional messages. The first step to use Message Queue for Apache Kafka is to create a topic for your application.
-   A consumer group identifies a type of consumers. The consumers subscribe to and consume the same type of messages by using the same consumption logic. The relationship between consumer groups and topics is N:N. One consumer group can subscribe to multiple topics, and one topic can be subscribed to by multiple consumer groups.

## Step 1: Create a topic

Perform the following steps to create a topic:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com/).

2.  In the top navigation bar, select the region where your instance is located.

    **Note:** You must create a topic in the region where your application is located. This means that you must select the region where the Elastic Compute Service \(ECS\) instance is deployed. A topic cannot be used across regions. For example, if a topic is created in the China \(Beijing\) region, the message producer and consumer must run on ECS instances in the China \(Beijing\) region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of your instance.

5.  In the left-side navigation pane, click **Topics**. On the page that appears, click **Create Topic**.

6.  In the **Create Topic** dialog box, set topic properties and then click **Create**.

    -   For Standard Edition instances:

        In the **Create Topic** dialog box, enter the topic name, select the instance of the topic, enter tags and the description, and then select the number of partitions. Then, click **Create**.

    -   For Professional Edition instances:

        In the **Create Topic** dialog box, enter the topic name, select the instance of the topic, enter tags and the description, and then select the number of partitions. Click **Advanced Settings**, set **Storage Engine** and **Message Type**, and then click **Create**.

        |Parameter|Description|
        |---------|-----------|
        |Storage Engine|Message Queue for Apache Kafka supports the following storage engines:         -   Cloud Storage: If this option is selected, disks provided by Alibaba Cloud are used and three replicas are stored in distributed mode. This type of storage engine features low latency, high performance, persistence, and high durability.
        -   Local Storage: If this option is selected, the in-sync replicas \(ISR\) algorithm of open source Apache Kafka is used and three replicas are stored in distributed mode.

**Note:** Only Message Queue for Apache Kafka Professional Edition instances that run Apache Kafka 2.2.0 support local storage. For more information about how to upgrade the edition of an instance, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

For more information about the comparison between the two storage engines, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md). |
        |cleanup.policy|If you select Local Storage, you must configure a log cleanup policy \(cleanup.policy\). Message Queue for Apache Kafka supports the following log cleanup policies:

        -   delete: the default message cleanup policy. If the remaining disk space is sufficient, messages are retained for the maximum retention period. If disk usage exceeds 85%, the disk space is insufficient, and earlier messages are deleted in advance to ensure service availability.
        -   compact: the [Apache Kafka log compaction policy](https://kafka.apache.org/documentation/?spm=a2c4g.11186623.2.15.1cde7bc3c8pZkD#compaction). Based on the log compaction policy, if the keys of the messages are the same, messages that have the latest key values are retained. This policy is applicable to scenarios where the system is recovered from a system failure or the cache is reloaded after system restart. For example, when you use Kafka Connect or Confluent Schema Registry, you must store the system status information or configuration information in a log-compacted topic.

**Note:** Log-compacted topics are generally used only in some ecosystem components, such as Kafka Connect or Confluent Schema Registry. In other components, do not set this attribute for a topic that is used to send and subscribe to messages. For more information, see [Message Queue for Apache Kafkademos](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master). |
        |Message Type|Message Queue for Apache Kafka supports the following message types:         -   Normal Message: By default, messages that have the same key are stored in the same partition in the order they are sent. If a broker in the cluster fails, the messages may be out of order.
        -   Partitionally Ordered Message: By default, messages that have the same key are stored in the same partition in the order they are sent. When a broker in the cluster fails, the messages are still stored in the same partition in the order they are sent. However, some messages in the partition cannot be sent until the partition is restored. |

    After the topic is created, it appears in the list on the **Topics** page.


## Step 2: Create a consumer group

Perform the following steps to create a consumer group:

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Consumer Groups**.

2.  On the **Consumer Groups** page, click the instance where your want to create a consumer group, and then click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, enter the consumer group name, select the instance of the consumer group, enter tags and the description, and then click **Create**.

    After the consumer group is created, it appears in the list on the **Consumer Groups** page.


Use the SDK to send and subscribe to messages based on the network type:

-   [Use the default endpoint to send and subscribe to messages](/intl.en-US/Quick-start/Step 4: Use the SDK to send and subscribe to messages/Use the default endpoint to send and subscribe to messages.md)
-   [Send and subscribe to messages by using an SSL endpoint with PLAIN authentication](/intl.en-US/Quick-start/Step 4: Use the SDK to send and subscribe to messages/Send and subscribe to messages by using an SSL endpoint with PLAIN authentication.md)

