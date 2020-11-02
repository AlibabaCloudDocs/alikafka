# View partition status

To view the total number of messages on the Message Queue for Apache Kafka broker or the consumption progress of each partition, you can query the partition status.

## Prerequisites

You have created a topic. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com) and select a region in the top navigation bar.
2.  In the left-side navigation pane, click **Topics**.
3.  On the Topics page, select the target instance, find the target topic, and then click **Partition Status** in the **Actions** column.

    ![partition_status](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2050549951/p57139.png)

    |Parameter|Description|
    |---------|-----------|
    |Total Messages on Server|The total number of messages in all partitions.|
    |Last Updated At|The time when the last message in all partitions is saved.|
    |Partition ID|The ID of the partition corresponding to the topic.|
    |Minimum Offset|The minimum message consumer offset of the topic in the current partition.|
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Last Updated At|The time when the last message in the partition is saved.|


