---
keyword: [kafka, query messages]
---

# Query messages

If an error occurs in message consumption, you can troubleshoot the error by querying the messages. In the Message Queue for Apache Kafka console, you can query messages by offset and by time.

## Background information

You can use one of the following methods to query messages:

-   If you can query logs to obtain the ID of the partition in the topic to which a message is sent and the message offset, we recommend that you query the message by offset. For more information, see [Query messages by offset](#section_x5a_pxt_51v).
-   If you do not know the message offset but know when the message is sent, we recommend that you query the message by time. For more information, see [Query messages by time](#section_qkk_rm7_sae).

## Precautions

-   The Message Queue for Apache Kafka console displays a maximum of 1 KB of content for each queried message. The excess content of the message is omitted. If you need to view the complete message, download the message.

    You can download messages only from Message Queue for Apache Kafka instances of Professional Edition. You can download up to 10 MB of messages at a time.

-   If you use an instance of Standard Edition, you can query a maximum of 10 messages of up to 256 KB in total size at a time.
    -   If the total size of the 10 queried messages exceeds 256 KB, only the first 256 KB of message content is displayed in the console.
    -   If the total size of the 10 queried messages is less than 256 KB, the message content is completely displayed. However, you can view only up to 10 messages. In this case, check the actual consumption data of the consumer.
-   If you use an instance of Professional Edition, you can query a maximum of 30 messages of up to 10 MB in total size at a time.

    -   If the total size of the 30 queried messages exceeds 10 MB, only the first 10 MB of message content is displayed in the console.
    -   If the total size of the 30 queried messages is less than 10 MB, the message content is completely displayed. However, you can view only up to 30 messages. In this case, check the actual consumption data of the consumer.
    For more information about instance editions, see [Billing](/intl.en-US/Pricing/Billing.md).

-   The query results are also related to the following message deletion policies of Message Queue for Apache Kafka:

    -   If the disk usage is lower than 85%, messages whose retention period expires are deleted at 04:00:00 every day.
    -   If the disk usage reaches 85%, messages whose retention period expires are immediately deleted.
    -   If the disk usage reaches 90%, messages are deleted from the earliest one stored in the Message Queue for Apache Kafka broker, no matter whether their retention period expires.
    **Note:** At least one storage file is retained by Message Queue for Apache Kafka when the messages are deleted. Therefore, when you query messages, the query results may contain messages whose retention period expires.


## Query messages by offset

In Message Queue for Apache Kafka, each offset maps a message. If you know the location of a message, you can specify an offset to query the message.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Message Query**.

6.  On the **Message Query** page, click the **Query by Offset** tab.

7.  Enter the name of the topic, select the partition, and enter the offset from which you want to query messages. Then, click **Search**.

    On the **Query by Offset** tab, messages that start from the specified offset are displayed. For example, if the specified partition and offset are both 5, the returned results are messages that start from Offset 5 in Partition 5.

    |Parameter|Description|
    |---------|-----------|
    |Partition|The ID of the partition in the topic to which the message is sent.|
    |Offset|The consumer offset of the message.|
    |Time|The timestamp of the producer when the message is sent or the value of the timestamp field that you specify in the `ProducerRecord` parameter. **Note:**

    -   If a value is specified for the timestamp field, the value is displayed.
    -   If no value is specified for the timestamp field, the system time when the message is sent is displayed.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot specify a value for the timestamp field on clients of Message Queue for Apache Kafka version 0.9 and earlier. |

8.  Click a message to view its details.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is converted to a string.|
    |Value|The content of the queried message. The content is converted to a string.|

9.  Click **Download Message** next to the Key or Value parameter to download the message.

    **Note:**

    -   This operation is supported only for instances of Professional Edition.
    -   You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.

## Query messages by time

You can query messages in all partitions by time. If you do not know the message offset but know the time range in which the messages are sent, you can query the messages by time. You can specify a point in time in the time range to query messages that are sent near this point.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Message Query**.

6.  On the **Message Query** page, click the **Query by Time** tab.

7.  Enter the name of the topic, select the partition, and specify a point in time from which you want to query messages. Then, click **Search**.

    On the **Query by Time** tab, the query results are displayed. The query results may vary based on the partition that you specify.

    -   If you select **All**, the messages in all partitions are displayed.
    -   If you specify a partition, the messages in the specified partition are displayed.
    |Parameter|Description|
    |---------|-----------|
    |Partition|The ID of the partition in the topic to which the message is sent.|
    |Offset|The consumer offset of the message.|
    |Time|The timestamp of the producer when the message is sent or the value of the timestamp field that you specify in the `ProducerRecord` parameter. **Note:**

    -   If a value is specified for the timestamp field, the value is displayed.
    -   If no value is specified for the timestamp field, the system time when the message is sent is displayed.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot specify a value for the timestamp field on clients of Message Queue for Apache Kafka version 0.9 and earlier. |

8.  Click a message to view its details.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is converted to a string.|
    |Value|The content of the queried message. The content is converted to a string.|

9.  Click **Download Message** next to the Key or Value parameter to download the message.

    **Note:**

    -   This operation is supported only for instances of Professional Edition.
    -   You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.

