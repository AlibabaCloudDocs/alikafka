---
keyword: [kafka, reset consumer offset]
---

# Reset consumer offsets

You can reset a consumer offset to change the current consumption position of a consumer. You can reset the consumer offset to skip the accumulated or undesired messages instead of consuming them, or to consume messages after a time point regardless of whether the messages before this time point have been consumed.

All consumers are stopped. Message Queue for Apache Kafka does not allow you to reset offsets of online consumers.

**Note:** After a consumer is stopped, the broker waits for a period of time, which is specified in `ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG` \(10,000 ms by default\), and then determines that the consumer has been stopped.

Message Queue for Apache Kafka supports the following modes of consumer offset resetting:

-   Clear messages: If a consumer does not want to consume accumulated messages on the broker any more, you can choose to clear messages for the consumer. This way, the consumption offset for the consumer is set to the latest position.

    **Note:** Accumulated messages are not deleted. Only the consumer offset is changed.

-   Start consumption at the specified time point: You can reset the offset of a consumer group to a time point "t" in the past or future, that is, a time point when a message is stored. Then, the consumer group subscribes to messages stored after "t".

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Consumer Groups**.

4.  On the top of the **Consumer Groups** page, click the instance for which you want to reset the consumer offset, find the consumer group, and then click **Reset Consumer Offset** in the **Actions**column.

5.  In the **Reset Consumer Offset** dialog box, select a topic from the **Topics** drop-down list, select a resetting policy, and then click **OK**.

    Message Queue for Apache Kafka supports the following resetting policies:

    -   **Clear all accumulated messages and consume messages from the latest offset.**: corresponds to the message clearing feature described at the beginning of this topic.

        ![clearall](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7050549951/p94025.png)

    -   **Reset Consumer Offset by Time**: corresponds to the feature of starting consumption at the specified time point described at the beginning of this topic. If you select this policy, you need to specify the time point.

        ![bytime](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7050549951/p94026.png)


