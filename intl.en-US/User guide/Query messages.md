---
keyword: [kafka, query messages]
---

# Query messages

If message consumption is abnormal at a time point, you can query the messages sent at that time point and the content of these messages. The Message Queue for Apache Kafka console provides two message query methods: query by offset and query by time, to improve the troubleshooting efficiency.

## Background

You can choose a query method based on the following recommendations:

-   If you can query logs to get the ID of the partition for the topic to which the message is sent and the message offset, we recommend that you [Query messages by offset](#section_x5a_pxt_51v).
-   If you do not know the message offset but know when it is sent, we recommend that you [Query messages by time](#section_qkk_rm7_sae).

## Precautions

-   A maximum of 1 KB content can be displayed in the console for each message. Content that exceeds 1 KB is automatically truncated. If you want to view the complete message, download the message.

    Only Message Queue for Apache Kafka instances of the Professional Edition allow you to download messages. Up to 10 MB messages can be downloaded.

-   If you use an instance of the Standard Edition, you can query a maximum number of 10 messages in up to 256 KB.
    -   If the size of 10 messages exceeds 256 KB, only the content within 256 KB is displayed in the console.
    -   If the size of 10 messages is less than 256 KB, at most 10 messages can be displayed in the console. See the actual consumption data of the consumer.
-   If you use an instance of the Professional Edition, you can query a maximum number of 30 messages in up to 10 MB.

    -   If the size of 30 messages exceeds 10 MB, only the content within 10 MB is displayed in the console.
    -   If the size of 30 messages is less than 10 MB, at most 30 messages can be displayed in the console. See the actual consumption data of the consumer.
    For more information about instance editions, see [Billing](/intl.en-US/Pricing/Billing.md).

-   Results that you can query are related to the message cleanup policy of Message Queue for Apache Kafka. Messages are cleared based on the following polices:
    -   Messages that exceed the retention period are deleted, but at least one storage file is retained. For example, if the message retention period is 72 hours, all messages that exceed 72 hours are cleared, but the last storage file is retained. Even if the messages in this file are all stored for more than 72 hours, the messages in this file can still be queried.
    -   If the total message size exceeds 85% of the disk capacity for message storage, the earliest message is deleted until the disk usage drops below 85%.

## Query messages by offset

In Message Queue for Apache Kafka, each offset maps a message. You can specify an offset to query the corresponding message after you determine the location of the message.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance whose messages you want to query is located.
3.  In the left-side navigation pane, click **Message Query**.
4.  On the **Message Query** page, click the instance and click the **Query by Offset** tab.
5.  In the three fields, enter the topic name, select a partition, enter the offset based on the instructions on the page, and then click **Search**.

    On the **Query by Offset** tab, messages are displayed starting from the specified offset. For example, if both the specified partition and offset are 5, the returned results are messages starting from the offset 5, as shown in the following figure.

    ![querybyoffset](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53123.png)

    |Parameter|Description|
    |---------|-----------|
    |Partitions|The partition for the topic of the messages.|
    |Offset|The consumer offset of the messages.|
    |TimeStamp|The value is the `timestamp` of the producer when the message is sent or that you specified in `ProducerRecord`. **Note:**

    -   If this field is set, the specified value is displayed.
    -   If this field is not set, the system time when the message is sent is used by default.
    -   If the value is 1970/x/x x:x:x, the sending time is set to 0 or an invalid value.
    -   This time cannot be set in Apache Kafka clients of 0.9 and earlier versions. |

6.  Optional. Click **Message Details** in the **Actions** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The size of the serialized message key or value. The value is `serializedKeySize/serializedValueSize` in `ConsumerRecord`.|
    |Value|The specific content of the queried message, which has been forcibly converted to a string.|

7.  Only for instances of the Professional Edition: Click **Download Message** next to the key or value to download the message.

    **Note:** You can download messages of up to 10 MB. Content exceeding 10 MB is not displayed.


## Query messages by time

You can query messages in all partitions by time. If you are not sure about the message location, but know the time range in which messages are sent, you can specify a time point in the time range to query messages near the message sending time point.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance whose messages you want to query is located.
3.  In the left-side navigation pane, click **Message Query**.
4.  On the **Message Query** page, click the instance and click the **Query by Time** tab.
5.  In the three fields, enter the topic name, select a partition, enter the time point, and then click **Search**.

    On the **Query by Time** tab, the search results are displayed. The partition value affects the search results.

    -   If you select **All**, the messages in all the partitions are displayed.

        ![querybytimeall](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53127.png)

    -   If you specify a partition, the messages in the specified partition are displayed.

        ![querybytimespecified](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53128.png)

    |Parameter|Description|
    |---------|-----------|
    |Partitions|The partition for the topic of the messages.     -   If you select a partition, the messages in the specified partition are displayed.
    -   If you select All, messages in partitions of the instance are randomly displayed. |
    |Offset|The consumer offset of the messages.|
    |TimeStamp|The value is the `timestamp` of the producer when the message is sent or that you specified in `ProducerRecord`. **Note:**

    -   If this field is set, the specified value is displayed.
    -   If this field is not set, the system time when the message is sent is used by default.
    -   If the value is 1970/x/x x:x:x, the sending time is set to 0 or an invalid value.
    -   This time cannot be set in Apache Kafka clients of 0.9 and earlier versions. |

6.  Optional. Click **Actions** in the **Message Details** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The size of the serialized message key or value. The value is `serializedKeySize/serializedValueSize` in `ConsumerRecord`.|
    |Value|The specific content of the queried message, which has been forcibly converted to a string.|

7.  Only for instances of the Professional Edition: Click **Download Message** next to the key or value to download the message.

    **Note:** You can download messages of up to 10 MB. Content exceeding 10 MB is not displayed.


