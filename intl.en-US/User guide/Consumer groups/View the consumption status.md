# View the consumption status

When message accumulation or skewing occurs, you can view the subscription relationship between the consumer group and the topic and determine the consumption progress of each partition.

## View consumer groups that subscribe to a topic

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).
2.  In the top navigation bar, select a region.
3.  In the left-side navigation pane, click **Topics**.
4.  On the Topics page, select the target instance, find the target topic, and in the **Actions** column, choose **More** \> **Subscription Relationship**.

    In the Subscription Relationship dialog box, all the consumer groups that subscribe to the topic are displayed.

    ![订阅关系 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8050549951/p94116.png)

5.  In the **Consumer Group ID** column, find the target Consumer Group, and in the **Actions** column, click **Details**.

    The message consumption details in each partition of this topic are displayed.

    ![订阅关系 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8050549951/p94121.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition corresponding to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to a specified topic are displayed in real time. **Note:**

    -   The value format is `<Client ID>_/<IP>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition, that is, the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. In this case, you need to analyze the running status of the consumer and improve the consumption speed. For more information, see [t998832.md\#](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


## View topics to which a consumer group subscribes

1.  Log on to the [Message Queue for Apache Kafkaconsole](https://kafka.console.aliyun.com/).
2.  In the top navigation bar, select a region.
3.  In the left-side navigation pane, click **Consumer Groups**.
4.  On the Consumer Groups page, click the target instance, find the target consumer group, and then click **Consumption Status** in the **Actions** column.

    In the Consumption Status dialog box, all the topics to which this consumer group subscribe and its **Total Messages Accumulated** and **Messages Accumulated** are displayed.

    ![消费状态 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9050549951/p94123.png)

5.  In the **Topic** column, find the target Consumer Group, and in the **Actions** column, click **Details**.

    The message consumption details in each partition of this topic are displayed.

    ![消费状态详情 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9050549951/p94114.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition corresponding to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to a specified topic are displayed in real time. **Note:**

    -   The value format is `<Client ID>_/<IP>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition, that is, the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. In this case, you need to analyze the running status of the consumer and improve the consumption speed. For more information, see [t998832.md\#](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


