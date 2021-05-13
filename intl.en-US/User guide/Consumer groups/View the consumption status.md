---
keyword: [kafka, consumption status]
---

# View the consumption status

When messages are accumulated or skewing occurs, you can view the subscriptions between consumer groups and topics and determine the status based on the consumption progress of each partition.

## View consumer groups that subscribe to a topic

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Topics**.

6.  On the **Topics** page, find the topic whose subscriptions you want to view, click the ![icon_more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6022597161/p185678.png) icon in the **Actions** column, and then select **Subscription Relationship**.

    In the **Subscription Relationship** dialog box, all the consumer groups that subscribe to the topic appear.

    ![Subscription](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8050549951/p94116.png)

7.  In the **Consumer Group** column, find the consumer group whose consumption status you want to view, and click **Details** in the **Actions** column.

    The message consumption details in each partition of the topic appear.

    ![Subscription](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8050549951/p94121.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition that corresponds to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to the topic are displayed in real time. **Note:**

    -   The value is in the format of `<Client ID>_/<IP address>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition. The value is equal to the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. In this case, you need to analyze the consumer running status and improve the consumption speed. For more information, see [Reset consumer offsets](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


## View topics to which a consumer group subscribes

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Consumer Groups**.

6.  On the **Consumer Groups** page, find the consumer group whose subscriptions you want to view, and click **Consumption Status** in the **Actions** column.

    In the **Consumption Status** dialog box, all the topics to which the consumer group has subscribed and the **Messages Accumulated** and **Last Consumed At** of each topic appear.

    ![Consumption status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9845180261/p267615.png)

7.  In the **Topic** column, find the topic whose consumption status you want to view, and click **Details** in the **Actions**column.

    The message consumption details in each partition of the topic appear.

    ![Consumption status details](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9845180261/p267654.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition ID|The ID of the partition that corresponds to the topic.|
    |owner|The ID and IP address of the online consumer that has subscribed to the topic are displayed in real time. **Note:**

    -   The value is in the format of `<Client ID>_/<IP address>`.
    -   You cannot view the owner information of offline consumers. |
    |Maximum Offset|The maximum message consumer offset of the topic in the current partition.|
    |Consumer Offset|The message consumer offset of the topic in the current partition.|
    |Messages Accumulated|The total number of accumulated messages in the current partition. The value is equal to the maximum offset minus the consumer offset. **Note:** Messages Accumulated is a key metric. If a large number of messages are accumulated, the consumer may be blocked or the consumption speed cannot keep up with the production speed. In this case, you need to analyze the consumer running status and improve the consumption speed. For more information, see [Reset consumer offsets](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md). |
    |Last Consumed At|The time when the last message consumed by the consumer group was sent to the broker for storage.|


