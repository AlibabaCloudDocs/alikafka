# Why is it not recommended to use a Go client developed with the Sarama library to send and subscribe to messages?

## Problem description

The following known problems exist with the Go client that is developed with the Sarama library:

-   When a partition is added to a topic, the Go client developed with the Sarama library cannot detect the new partition or consume messages from the partition. Only after the client is restarted, it can consume messages from the new partition.
-   When the client subscribes to more than two topics at the same time, the client may fail to consume messages from some partitions.
-   If the policy for resetting consumer offsets of the Go client developed with the Sarama library is set to `Oldest(earliest)`, the client may start to consume messages from the earliest offset when the client breaks down or the broker version is upgraded. This is because the out\_of\_range class is implemented in the client.

1.  We recommend that you substitute the Go client developed by Confluent for the Go client developed with the Sarama library at the earliest opportunity.

    For more information about the demo address of the Go client developed by Confluent, visit [kafka-confluent-go-demo](https://github.com/AliwareMQ/aliware-kafka-demos/tree/master/kafka-confluent-go-demo).

    **Note:** If you cannot replace the client in a short period of time, take note of the following items:

    -   In a production environment, set the policy for resetting consumer offsets to `Newest(latest)`. In a test environment or scenarios where messages can be repeatedly received, set the policy for resetting consumer offsets to `Oldest(earliest)`.
    -   If the consumer offset is reset and a large number of messages are accumulated, you can manually reset the consumer offset to the offset at a specific point in time in the Message Queue for Apache Kafka console. This frees you from modifying the client code or changing the consumer group. For more information about how to reset consumer offsets, see [Reset consumer offsets](/intl.en-US/Console Guide/Consumer groups/Reset consumer offsets.md).

