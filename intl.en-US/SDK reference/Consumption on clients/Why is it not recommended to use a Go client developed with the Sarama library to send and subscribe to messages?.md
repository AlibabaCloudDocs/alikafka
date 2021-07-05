# Why is it not recommended to use a Go client developed with the Sarama library to send and subscribe to messages?

## Problem description

The following known problems exist with the Go client developed with the Sarama library:

-   When a partition is added to a topic, the Go client developed with the Sarama library cannot detect the new partition or consume messages from the partition. Only after the client is restarted, it can consume messages from the new partition.
-   When the client subscribes to more than two topics at the same time, the client may fail to consume messages from some partitions.
-   When the client whose consumer offset is set to `Oldest(earliest)` breaks down, the client may start to consume messages from the earliest offset because the out\_of\_range class is implemented in the client.

1.  We recommend that you substitute the Go client developed by Confluent for the Go client developed with the Sarama library at the earliest opportunity.

    For more information about the demo address of the Go client developed by Confluent, visit [kafka-confluent-go-demo](https://github.com/AliwareMQ/aliware-kafka-demos/tree/master/kafka-confluent-go-demo).


