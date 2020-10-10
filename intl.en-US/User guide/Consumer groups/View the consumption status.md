# View the consumption status

When message accumulation or skewing occurs, you can view the subscription relationship between the consumer group and the topic and determine the status based on the consumption progress of each partition.

## View consumer groups that subscribe to a topic

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the top navigation bar, select the target region.

3.  In the left-side navigation pane, click **Topics**.

4.  On the **Topics** page, select the target instance, find the target topic, and choose **More** \> **Subscription Relationship** in the **Actions** column.

    In the **Subscription Relationship** dialog box, all the consumer groups that subscribe to the topic are displayed.

    ![Subscription relationship](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8050549951/p94116.png)

5.  In the **Consumer Group** column, find the target consumer group and click **Details** in the **Actions** column.

    The message consumption details in each partition of the topic are displayed.

    ![Subscription relationship](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8050549951/p94121.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition corresponding to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to a specified topic are displayed in real time. **Note:**

    -   The value format is `<Client ID>_/<IP address>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition, that is, the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. If this is the case, you need to analyze the consumer running status and improve the consumption speed. For more information, see [Reset consumer offsets](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


## View topics to which a consumer group subscribes

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the top navigation bar, select the target region.

3.  In the left-side navigation pane, click **Consumer Groups**.

4.  On the **Consumer Groups** page, select the target instance, find the target consumer group, and then click **Consumption Status** in the **Actions** column.

    In the **Consumption Status** dialog box, all the topics to which the consumer group has subscribed and the **Messages Accumulated**and **Last Consumed At** of each topic are displayed.

    ![Consumption Status](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9050549951/p94123.png)

5.  In the **Topic** column, find the target topic and click **Details** in the **Actions**column.

    The message consumption details in each partition of the topic are displayed.

    ![Consumption status details](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9050549951/p94114.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition corresponding to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to a specified topic are displayed in real time. **Note:**

    -   The value format is `<Client ID>_/<IP address>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition, that is, the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. If this is the case, you need to analyze the consumer running status and improve the consumption speed. For more information, see [Reset consumer offsets](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


