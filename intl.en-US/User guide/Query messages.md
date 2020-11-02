# Query messages

If message consumption is abnormal at a certain time point, you can query which messages were sent at that time point and the content of these messages. The Message Queue for Apache Kafka console provides two message query methods: query by offset and query by time, to improve the troubleshooting efficiency.

## Background

You can query messages according to the following recommended methods:

-   If you can query logs to get the ID of the partition to which the message is sent and the message offset, we recommend that you use [Query by offset](#section_x5a_pxt_51v).

-   If you do not know the message offset but know when it is sent, we recommend that you use [Query by time](#section_qkk_rm7_sae).


**Note:**

-   A maximum of 1 KB content can be displayed in the console for each message. Content that exceeds 1 KB is automatically truncated. If you want to view the complete message, download the message.

    Currently, Message Queue for Apache Kafka instances of only the Professional Edition allow you to download messages of up to 10 MB. [Buy Now\>\>](https://common-buy.aliyun.com/?spm=5176.kafka.Index.1.246025e8fV8VQT&commodityCode=alikafka_pre&regionId=cn-hangzhou#/buy)

-   If you use an instance of the Standard Edition, you can query up to 256 KB messages and display up to 10 messages.

    -   If the size of 10 messages exceeds 256 KB, only the content within 256 KB is displayed in the console.

    -   If the size of 10 messages is less than 256 KB, up to 10 messages are displayed in the console. See the actual consumption data of the consumer.

-   If you use an instance of Professional Edition, you can query up to 10 MB messages and display up to 30 messages.

    -   If the size of 30 messages exceeds 10 MB, only the content within 10 MB is displayed in the console.

    -   If the size of 30 messages is less than 10 MB, up to 30 messages are displayed in the console. See the actual consumption data of the consumer.

    For more information about the instance version, see [Billing](/intl.en-US/Pricing/Billing.md).

-   Results that you can query is related to the information cleanup policy of Message Queue for Apache Kafka. The cleanup policy is described as follows:

    -   Messages that exceed the storage duration are deleted, but at least one storage file is retained. For example, if the message storage duration is 72 hours, all messages that exceed 72 hours are cleared, but the last storage file is retained. Even if the messages in this file are all stored for more than 72 hours, the messages in this file can still be queried.

    -   If the total message size exceeds 85% of the capacity of the message storage disk, the earliest message is cleared until the remaining capacity drops below 85%.


## Query by offset

In Message Queue for Apache Kafka, an offset maps a message. You can specify an offset to query the corresponding message after determining the location of the message to be queried.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance you want to query is located.
2.  In the left-side navigation pane, click **Message Query**. On the Message Query page, find the target instance, and click the **Query by Offset** tab.

3.  In the three fields, enter the topic name, select the target partition, enter the offset according to the instructions on the page, and then click **Search**.

    On the **Query by Offset** tab, up to 10 messages starting from the specified offset are displayed. For example, if both the specified partition and offset are 5, the returned results are messages starting from the offset 5, as shown in the following figure:

    ![querybyoffset](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53123.png)

    Eight messages are returned, indicating that the maximum offset of the partition is 12 or that the size of the returned messages exceeds 256 KB.

    The fields in the search result are described as follows:

    -   **Partition**: The value is same as the partition ID you selected in Step 3.

    -   **Offset**: Up to 10 offsets starting from the offset you specified in Step 3 are displayed.

    -   **TimeStamp**: The value is the `timestamp` of the producer when the message is sent or that you specified in `ProducerRecord`. If this field is not configured, the system time when the message is sent is used by default. If this field is configured, it is displayed based on the configured value. If the value is 1970/x/x x:x:x, the sending time is set to 0 or an invalid value. This time cannot be set in Apache Kafka clients of 0.9 and earlier versions.

4.  Optional: Click **Message Details** in the **Actions** column. The fields are described as follows:

    -   **Key\(size:XXB\)**: the size of the serialized message key or value. The value is `serializedKeySize/serializedValueSize` in `ConsumerRecord`.

    -   **Value\(size:XXB\)**: the specific content of the queried message, which has been forcibly converted to a string.

5.  **For Professional Edition only**: Click **Download Message** next to the key or value to download the message. You can download messages of up to 10 MB. Content exceeding 10 MB is not displayed.


## Query by time

You can query messages in all partitions by time. If you are not sure about the message location, but know the time range in which messages are sent, you can specify a time point in the time range to query messages near the message sending time point.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance you want to query is located.

2.  In the left-side navigation pane, click **Message Query**. On the Message Query page, find the target instance, and click the **Query by Time** tab.

3.  In the three fields, enter the topic name, select the target partition, enter the time point, and then click **Search**.

    On the **Query by Time** tab, the search result is displayed. The partition value affects the search result.

    -   If you select **All** for **Partition**, messages near the specified time point in the randomly displayed partition of the instance are displayed. A total of 10 messages are displayed at most. For example, if you select **All** for Partition and set the time to 2019-05-07 00:00:00, the search result is shown in the following figure.

        ![querybytimeall](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53127.png)

    -   If a partition is specified, up to 10 messages near the specified time point in the partition are displayed. For example, if you select 5 for Partition and set the time to 2019-05-07 00:00:00, the search result is shown in the following figure.

        ![querybytimepartition](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53128.png)

    The fields in the search result are described as follows:

    -   **Partition**: The value displayed depends on whether a partition is specified in Step 3. If **All** is selected, partitions of the instance are randomly displayed.

    -   **Offset**: If you specify a partition in Step 3, the offsets near the specified time point in this partition are displayed. If **All** is selected for Partition in Step 3, offsets near the specified time point in any partition of the instance are randomly displayed. A maximum of 10 offsets are displayed regardless of whether a partition is specified.

    -   **TimeStamp**: The value is the `timestamp` of the producer when the message is sent or that you specified in `ProducerRecord`. If this field is not configured, the system time when the message is sent is used by default. If this field is configured, it is displayed based on the configured value. If the value is 1970/x/x x:x:x, the sending time is set to 0 or an invalid value. This time cannot be set in Apache Kafka clients of 0.9 and earlier versions.

4.  Optional: Click **Message Details** in the **Actions** column. The fields are described as follows:

    -   **Key\(size:XXB\)**: the size of the serialized message key or value. The value is `serializedKeySize/serializedValueSize` in `ConsumerRecord`.

    -   **Value\(size:XXB\)**: the specific content of the queried message, which has been forcibly converted to a string.

5.  **For Professional Edition only**: Click **Download Message** next to the key or value to download the message. You can download messages of up to 10 MB. Content exceeding 10 MB is not displayed.


