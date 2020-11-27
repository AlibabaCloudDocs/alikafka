---
keyword: [Kafka, query messages]
---

# Query messages

If an exception occurs in message consumption at a specific time, you can query the messages that are sent at that time. You can also view the content of the messages. The Message Queue for Apache Kafka console allows you to query messages by offset and by time. This allows you to troubleshoot issues at the earliest opportunity.

## Background information

You can choose a query method based on the following recommendations:

-   Each message is sent to a topic. If you can query logs to obtain the ID of the partition in the topic and the message offset, we recommend that you query messages by offset. For more information, see [Query messages by offset](#section_x5a_pxt_51v).
-   If you do not know the message offset but know when the message is sent, we recommend that you query messages by time. For more information, see [Query messages by time](#section_qkk_rm7_sae).

## Usage notes

-   The Message Queue for Apache Kafka console displays a maximum of 1 KB of content for each queried message. The excess content of the message is omitted. If you need to view the complete message, download the message.

    Only Message Queue for Apache Kafka instances of Professional Edition allow you to download messages. You can download up to 10 MB of messages at a time.

-   If you use an instance of Standard Edition, you can query a maximum of 10 messages of up to 256 KB in total size at a time.
    -   If the total size of the 10 queried messages exceeds 256 KB, only the first 256 KB of message content can be retrieved.
    -   If the total size of the 10 queried messages is less than 256 KB, the message content is completely retrieved. However, you cannot view more messages by using this query. In this case, check the actual consumption data of the consumer.
-   If you use an instance of Professional Edition, you can query a maximum of 30 messages of up to 10 MB in total size at a time.

    -   If the total size of the 30 queried messages exceeds 10 MB, only the first 10 MB of message content can be retrieved.
    -   If the total size of the 30 queried messages is less than 10 MB, the message content is completely retrieved. However, you cannot view more messages by using this query. In this case, check the actual consumption data of the consumer.
    For more information about instance editions, see [Billing](/intl.en-US/Pricing/Billing.md).

-   The query results are also related to the following message clearance policies of Message Queue for Apache Kafka:

    -   If the disk usage is lower than 85%, expired messages are deleted at 04:00 every day.
    -   If the disk usage reaches 85%, expired messages are immediately deleted.
    -   If the disk usage reaches 90%, stored messages \(expired or not\) are deleted in the sequence that they were stored in the clients.
    **Note:** At least one storage file is retained by Message Queue for Apache Kafka when the messages are deleted. Therefore, when you query messages, the query results may contain messages have exceeded the retention period.


## Query messages by offset

In Message Queue for Apache Kafka, each offset maps a message. If you know the location of a message, you can specify an offset to query the message.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region of the instance from which you want to query messages.
3.  In the left-side navigation pane, click **Message Query**.
4.  On the **Message Query** page, select the instance. The **Query by Offset** tab appears.
5.  Enter the topic name, select the partition, enter the offset, and then click **Search**.

    On the **Query by Offset** tab, messages that start from the specified offset are displayed. For example, if the specified partition and offset are 5, the returned results are messages that start from Offset 5 in Partition 5. The following figure shows the query results.

    ![querybyoffset](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4450549951/p53123.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition|The ID of the partition in the topic to which the message is sent.|
    |Offset|The consumer offset of the message.|
    |TimeStamp|The timestamp of the producer when the message is sent or the value of the `timestamp` field that you specify in the `producer record`. **Note:**

    -   If you have specified the timestamp field, the specified value is displayed.
    -   If you have not specified the timestamp field, the system time when the message is sent is displayed.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot specify the timestamp field on clients of Apache Kafka version 0.9 and earlier. |

6.  Optional. Click **Message Details** in the **Actions** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is converted to a string.|
    |Value|The content of the queried message. The content is converted to a string.|

7.  Click **Download Message** next to the Key or Value parameter to download the message. This operation is supported only for instances of Professional Edition.

    **Note:** You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.


## Query messages by time

You can query messages in all partitions by time. If you do not know the message offset but know the time range in which the messages are sent, you can query the messages by time. You can specify a time in the time range to query messages that are sent near that time.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select the region of the instance from which you want to query messages.

3.  In the left-side navigation pane, click **Message Query**.


1.  2.  3.  4.  On the **Message Query** page, select the instance. Then, click the **Query by Time** tab.
5.  Enter the topic name, select a partition, specify a time, and then click **Search**.

    On the **Query by Time** tab, the query results are displayed. The query results may vary based on the partition that you specify.

    -   If you select **All**, the messages in all partitions are displayed.

        ![querybytimeall](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4450549951/p53127.png)

    -   If you specify a partition, the messages in the specified partition are displayed.

        ![querybytimespecified](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4450549951/p53128.png)

    |Parameter|Description|
    |---------|-----------|
    |Partition|The ID of the partition in the topic to which the message is sent.     -   If you specify a partition, the messages in the specified partition are displayed.
    -   If you select All, the messages in all partitions are randomly displayed. |
    |Offset|The consumer offset of the message.|
    |TimeStamp|The timestamp of the producer when the message is sent or the value of the `timestamp` field that you specify in the `producer record`. **Note:**

    -   If you have specified the timestamp field, the specified value is displayed.
    -   If you have not specified the timestamp field, the system time when the message is sent is displayed.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot specify the timestamp field on clients of Apache Kafka version 0.9 and earlier. |

6.  Optional. Click **Message Details** in the **Actions** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is converted to a string.|
    |Value|The content of the queried message. The content is converted to a string.|

7.  Click **Download Message** next to the Key or Value parameter to download the message. This operation is supported only for instances of Professional Edition.

    **Note:** You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.


