---
keyword: [kafka, consumption, consumer, network bandwidth]
---

# Why does a consumer slowly pull a message from the broker or fail to pull a message from the broker?

These issues may occur due to one of the following reasons: The consumption traffic reaches the network bandwidth, the traffic for a single message exceeds the network bandwidth, and the traffic for the messages pulled by the consumer at a time exceeds the network bandwidth.

## Condition

Topics to which the consumer subscribes receive messages, and the consumption does not reach the latest offset. However, the consumer slowly pulls messages from the broker or fails to pull messages from the broker. Such issues occur more frequently when the consumption is performed over the Internet.

## Cause

These issues may occur due to the following reasons:

-   The consumption traffic reaches the network bandwidth.
-   The traffic for a single message exceeds the network bandwidth.
-   The traffic for the messages pulled by the consumer at a time exceeds the network bandwidth.

    **Note:** The following parameters determine the number and size of messages that a consumer can pull at a time:

    -   max.poll.records: the maximum number of messages that the consumer can pull at a time.
    -   fetch.max.bytes: the maximum number of bytes of messages that the consumer can pull at a time.
    -   max.partition.fetch.bytes: the maximum number of bytes of messages that the consumer can pull from a single partition at a time.

## Remedy

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.10.22f150ddqNXasY) to query messages.

    If messages are returned, perform the subsequent steps.

2.  On the **Instances** page, click **Monitoring** in the left-side navigation pane to check whether the consumption traffic reaches the network bandwidth.

    If the consumption traffic reaches the network bandwidth, increase the network bandwidth.

3.  Check whether the traffic for a single message in the topic exceeds the network bandwidth.

    If the traffic exceeds the network bandwidth, increase the network bandwidth or reduce the size of the message.

4.  Check whether the traffic for the messages pulled by the consumer at a time exceeds the network bandwidth.

    If the traffic exceeds the network bandwidth, adjust the configuration of the following parameters:

    -   fetch.max.bytes: Set the parameter to a value smaller than the network bandwidth.
    -   max.partition.fetch.bytes: Set the parameter to a value smaller than the limit value. The limit value is calculated by using the following formula: Network bandwidth/Number of partitions that are subscribed to.

