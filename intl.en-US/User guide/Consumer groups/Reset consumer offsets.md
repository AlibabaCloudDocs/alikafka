# Reset consumer offsets

You can reset a consumer offset to change the current consumption position of a consumer. You can reset the consumer offset to skip the accumulated or undesired messages instead of consuming them, or to consume messages after a certain time point regardless of whether the messages before this time point have been consumed or not.

## Background

Specifically, the following two functions are supported:

-   Clear messages: For some reason, the subscriber does not want to consume accumulated messages on the broker any longer. In this case, the subscriber can clear messages and specify the consumption offset to the latest position.

    **Note:** Accumulated messages are not deleted. Only the consumption offset is changed.

-   Start consumption at the specified time point: You can reset the offset of a consumer group to a previous or future time point t \(a time point when a message is stored\). After the reset, the consumer group subscribes to messages stored after the "t" time point.

## Prerequisites

You have stopped all consumers. Message Queue for Apache Kafka does not support resetting offsets of online consumers.

**Note:** After the time specified in `ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG` \(10,000 ms by default\) elapses since the consumer has been stopped, the broker determines that the consumer has been stopped.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select a region where the instance is located, such as **China \(Hangzhou\)**.
2.  In the left-side navigation pane, click **Consumer Groups**.
3.  On the Consumer Groups page, enter the consumer group name in the search box, and then click **Search**.
4.  On the Consumer Groups page, click **Reset Consumer Offset** in the **Actions** column of the corresponding consumer group.
5.  In the dialog box that appears, select the reset policy, and then click **OK**.

    Two reset policies are available:

    -   **Clear all accumulated messages and consume messages from the latest offset**: It corresponds to the function of clearing messages described at the beginning of this topic.

        ![clearall ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7050549951/p94025.png)

    -   **Reset Consumer Offset by Time**: It corresponds to the function of starting consumption at the specified time point described at the beginning of this topic. If you select this option, specify a time point.

        ![bytime ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7050549951/p94026.png)


