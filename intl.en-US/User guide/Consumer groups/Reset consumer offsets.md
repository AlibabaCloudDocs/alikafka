---
keyword: [kafka, reset consumer offset]
---

# Reset consumer offsets

You can reset a consumer offset to change the position from which a consumer starts to consume messages. You can reset the consumer offset to skip the accumulated or undesired messages instead of consuming them, or to consume messages stored after a point in time regardless of whether the messages stored before this point in time are consumed.

All consumers are stopped. Message Queue for Apache Kafka does not allow you to reset offsets of online consumers.

**Note:** After a consumer is stopped, the broker waits for a period of time and then determines that the consumer has been stopped. This period of time is specified in `ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG` and is 10,000 ms by default.

Message Queue for Apache Kafka allows you to reset consumer offsets in one of the following ways:

-   Clear messages: If a consumer no longer wants to consume accumulated messages on the broker, you can choose to clear messages for the consumer. This way, the consumer offset for the consumer is set to the latest position.

    **Note:** Accumulated messages are not deleted. Only the consumer offset is changed.

-   Start consumption at a specified point in time: You can reset the offset of a consumer group to a point in time "t" that is in the past or future. The point in time "t" is a point in time when a message is stored. Then, the consumer group subscribes to messages stored after "t".

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Consumer Groups**.

6.  On the **Consumer Groups** page, find the consumer group whose consumer offset you want to reset, and click **Reset Consumer Offset** in the **Actions** column.

7.  In the **Reset Consumer Offset** dialog box, select a topic from the **Topics** drop-down list, select a resetting policy, and then click **OK**.

    Message Queue for Apache Kafka supports the following resetting policies:

    -   **Clear all accumulated messages and consume messages from the latest offset.**: corresponds to the message clearing feature described at the beginning of this topic.

        ![Clear all accumulated messages](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5475180261/p267598.png)

    -   **Reset Consumer Offset by Time**: corresponds to the feature of starting consumption at a specified point in time. This feature is described at the beginning of this topic. If you select this policy, you must specify a point in time.

        ![Reset consumer offsets by time](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5475180261/p267600.png)


