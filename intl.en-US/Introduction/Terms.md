# Terms

This topic describes the terms used in Message Queue for Apache Kafka. This helps you better understand and use Message Queue for Apache Kafka.

-   **Apache Kafka**

    A distributed, open source platform for data stream processing. This platform allows you to publish, subscribe to, store, and process data streams in real time. For more information, see [Apache Kafka](https://kafka.apache.org/).

-   **Message Queue for Apache Kafka**

    A fully managed Apache Kafka service provided by Alibaba Cloud. This service frees you from deployments and O&M, and offers you a cost-effective service that features high scalability, reliability, and throughput. For more information, see [What is Message Queue for Apache Kafka?](/intl.en-US/Introduction/What is Message Queue for Apache Kafka?.md).

-   **Zookeeper**

    A distributed, open source coordination service for applications. In Message Queue for Apache Kafka, ZooKeeper is used to manage clusters and configurations, and elect leaders. ZooKeeper is part of the Message Queue for Apache Kafka architecture. You do not need to learn about ZooKeeper.

-   **Broker**

    A node server in a Message Queue for Apache Kafka cluster. Message Queue for Apache Kafka provides a fully managed service that automatically changes the broker quantity and configuration based on the traffic specifications of your instances. You do not need to learn about the specific broker information.

-   **Cluster**

    A collection of multiple brokers.

-   **Instance**

    An independent Message Queue for Apache Kafka resource entity that corresponds to a cluster.

-   **VPC-connected instance**

    An instance that provides only a VPC endpoint, and can be accessed only from the VPC.

-   **Internet- and VPC-connected instance**

    An Internet- and VPC-connected instance that provides public and VPC endpoints, and can be accessed from the Internet and the VPC.

-   **Major version upgrade**

    An upgrade across versions. For example, you can upgrade a Message Queue for Apache Kafka instance from version 0.10.x to version 2.x. For more information, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

-   **Minor version upgrade**

    An upgrade that does not cross versions. For example, you can upgrade a Message Queue for Apache Kafka instance from version 0.10 to version 0.10.2, or from version 0.10.2 to the 0.10.2 kernel-optimized version. For more information, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

-   **Endpoint**

    An address that is used for producers or consumers to connect to Message Queue for Apache Kafka.This address consists of the broker URL and port number. For more information, see [Comparison between endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).

-   **Message**

    A carrier for transferring information in Message Queue for Apache Kafka. Messages can be page views, server logs, and the information about system resources, such as CPU and memory resources. In Message Queue for Apache Kafka, a message is a byte array.

-   **Message retention period**

    The maximum message retention period when the disk capacity is sufficient.

    -   When the disk usage reaches 85%, the disk capacity is insufficient, and the system deletes messages from the earliest stored ones to ensure service availability.
    -   The default value is 72 hours. Valid values: 24 hours to 480 hours.
-   **Maximum message size**

    The maximum size of a message that you can send and receive in Message Queue for Apache Kafka.

    -   The maximum message size is 10 MB for instances of the Standard Edition and Professional Edition.
    -   Before you modify the configuration, ensure that the new value matches the configuration on the producer and consumer.
-   **Publish/subscribe pattern**

    An asynchronous communication model between services. A publisher sends a message directly to a specified topic, and does not need to know the subscriber that receives the message. A subscriber receives a message directly from a specified topic, and does not need to know the publisher that sends the message. Message Queue for Apache Kafka uses the publish/subscribe pattern. For more information, see [Publish/subscribe pattern of Message Queue for Apache Kafka](/intl.en-US/Introduction/Architecture.md).

-   **Subscription relationship**

    The subscription relationship between consumer groups and topics. In Message Queue for Apache Kafka, you can query the status of online consumer groups that have subscribed to a specified topic. The status of the offline consumer groups cannot be queried.

-   **Producer**

    An application that sends messages to Message Queue for Apache Kafka.

-   **Consumer**

    An application that receives messages from Message Queue for Apache Kafka.

-   **Consumer Group**

    A group of consumers that have the same group ID. When a topic is consumed by multiple consumers in the same consumer group, each message in the topic is delivered only to one consumer. This balances the load over consumers and allows messages in a topic to be concurrently consumed.

-   **Topic**

    A message topic that is used to classify messages.

-   **Topic traffic rebalance**

    An operation that evenly redistributes the topic traffic after a Message Queue for Apache Kafka cluster is scaled out. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md).

-   **Partition**

    A message partition that is used to store messages. A topic consists of one or more partitions. Messages in each partition are stored on one or more brokers.

-   **Offset**

    A sequence number that is assigned to a message when the message arrives at a partition.

-   **Minimum offset**

    The minimum offset in a partition. This is the offset for the first message in the current partition. For more information about how to query the minimum offset in the current partition, see [View partition status](/intl.en-US/User guide/Topics/View partition status.md).

-   **Maximum offset**

    The maximum offset in a partition. This is the offset for the latest message in the current partition. For more information about how to query the maximum offset in the current partition, see [View partition status](/intl.en-US/User guide/Topics/View partition status.md).

-   **Consumer offset**

    The maximum offset of messages that are consumed in a partition. For more information about how to query the consumer offset, see [View the consumption status](/intl.en-US/User guide/Consumer groups/View the consumption status.md).

-   **Latest consumption time**

    The time when the latest message consumed by a consumer group was published to and stored on the Message Queue for Apache Kafka broker. If no message accumulation occurs, the time is close to the message sending time.

-   **Message accumulation**

    The total number of accumulated messages in the current partition. The value is equal to the maximum offset minus the consumer offset. Message accumulation is a key metric. If a large number of messages are accumulated, consumers may be blocked, or the consumption speed cannot keep up with the production speed. In this case, you must analyze the running status of consumers and improve the consumption speed. You can clear all accumulated messages, and start to consume from the maximum offset or reset consumer offsets based on time points. For more information, see [Reset consumer offsets](https://help.aliyun.com/document_detail/68329.html#task-68329-zh).

-   **Local storage**

    A storage engine that uses the In-Sync Replicas \(ISR\) algorithm of native Apache Kafka. If you have special requirements, such as compact, idempotence, transactions, and partitionally ordered messages, we recommend that you use local storage. For more information, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

-   **Cloud storage**

    A storage engine that uses the algorithm for Alibaba Cloud disks. Cloud storage has the benefits of the underlying storage of Alibaba Cloud. Compared with local storage, cloud storage provides better performance in auto scaling, reliability, availability, and cost-effectiveness. Therefore, in most cases, we recommend that you use cloud storage. For more information, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

-   **cleanup.policy**

    A log cleanup policy. Configure a log cleanup policy only if you use local storage. Message Queue for Apache Kafka supports the following log cleanup policies:

    -   delete: It is the default message cleanup policy. If the remaining disk space is sufficient, messages are retained for the maximum retention period. If the remaining disk space is insufficient \(the disk usage exceeds 85%\), historical messages are deleted to ensure service availability.
    -   compact: The [Apache Kafka log compaction policy](https://kafka.apache.org/documentation/?spm=a2c4g.11186623.2.15.1cde7bc3c8pZkD#compaction) is used. Based on the log compaction policy, if the keys of the messages are the same, messages that have the latest key values are retained. This policy is applicable to scenarios where the system is recovered from a system failure or the cache is reloaded after system restart. For example, when you use Kafka Connect or Confluent Schema Registry, you must store the system status information or configuration information in a log compacted topic.

        **Note:** Log compacted topics are generally used only in some ecosystem components, such as Kafka Connect or Confluent Schema Registry. Do not set this attribute for a topic used to send and subscribe to messages in other components. For more information, see [Message Queue for Apache Kafka demos](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master).

-   **Normal message**

    By default, messages of the same key are stored in the same partition in the order in which they are sent. A small number of messages are out of order during cluster restart or downtime. For more information, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

-   **Partitionally ordered messages**

    By default, messages of the same key are stored in the same partition in the order in which they are sent. When a cluster failure occurs, the messages are still in order. However, messages in some partitions cannot be sent until the failed instance is restored. For more information, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

-   **Connector**

    A component in Message Queue for Apache Kafka, which is used to synchronize data between Message Queue for Apache Kafka and other Alibaba Cloud services. For more information, see [Overview](/intl.en-US/User guide/Connectors/Overview.md).

-   **Tag**

    A tag that is used to identify Message Queue for Apache Kafka resources. You can use tags to classify Message Queue for Apache Kafka resources based on resource features for easy resource search and aggregation. For more information, see [Overview](/intl.en-US/User guide/Tags/Overview.md).

-   **RAM**

    A service provided by Alibaba Cloud to manage user identities and resource access permissions. You can only grant permissions to RAM users in the Message Queue for Apache Kafka console or by using the API operations. No matter whether RAM users are authorized or not, RAM users can use SDKs to send and receive messages. For more information, see [Access control overview](/intl.en-US/Access control/Overview.md).

-   **ACL**

    A service provided by Message Queue for Apache Kafka to manage the permissions of SASL users and clients to send and receive messages by using SDKs. This feature is consistent with that in open source Apache Kafka. The ACL feature is only applicable to scenarios where you want to implement access control for users that use SDKs to send and receive messages. This feature is not applicable to scenarios where you want to implement access control for users that send and receive messages in the Message Queue for Apache Kafka console or by using API operations. For more information, see [Access control overview](/intl.en-US/Access control/Overview.md).


