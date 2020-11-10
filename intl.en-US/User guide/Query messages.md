---
keyword: [kafka, query messages]
---

# Query messages

If message consumption is abnormal at a time point, you can query the messages sent at that time point and the content of these messages. The Message Queue for Apache Kafka console allows you to query messages by offset and by time to improve the troubleshooting efficiency.

## Background information

You can choose a query method based on the following recommendations:

-   If you can query logs to obtain the ID of the partition for the topic to which the message is sent and the message offset, we recommend that you query messages by offset. For more information, see [Query messages by offset](#section_x5a_pxt_51v).
-   If you do not know the message offset but know when it is sent, we recommend that you query messages by time. For more information, see [Query messages by time](#section_qkk_rm7_sae).

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

-   Query results are related to the message cleanup policies of Message Queue for Apache Kafka. Messages are cleaned up based on the following polices:
    -   When disk usage is less than 85%, expired messages are deleted at 04:00 every day.
    -   When disk usage reaches 85%, expired messages are immediately deleted.
    -   When disk usage reaches 90%, stored messages are deleted in sequence from the earliest ones, no matter whether they are expired or not.

## Query messages by offset

In Message Queue for Apache Kafka, each offset maps a message. You can specify an offset to query the corresponding message after you determine the location of the message.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance from which you want to query messages is located.
3.  In the left-side navigation pane, click **Message Query**.
4.  On the **Message Query** page, select the instance. The **Query by Offset** tab appears.
5.  Enter the topic name, select the partition, enter the offset, and then click **Search**.

    On the **Query by Offset** tab, messages are displayed starting from the specified offset. For example, if both the specified partition and offset are 5, the returned results are messages starting from Offset 5 in Partition 5, as shown in the following figure.

    ![querybyoffset](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53123.png)

    |Parameter|Description|
    |---------|-----------|
    |Partitions|The ID of the partition for the topic to which the message is sent.|
    |Offset|The consumer offset of the message.|
    |TimeStamp|The timestamp of the producer when the message is sent or the value of the `timestamp` field that you specify in the `producer record`. **Note:**

    -   If you set the timestamp field, the specified value is displayed.
    -   If you do not set the timestamp field, the system time when the message is sent is used.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot set the timestamp field on clients of Apache Kafka version 0.9 and earlier. |

6.  Optional. Click **Message Details** in the **Actions** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is forcibly converted to a string.````|
    |Value|The content of the queried message. The content is forcibly converted to a string.|

7.  Click **Download Message** next to the Key or Value parameter to download the message. This operation is supported only for instances of Professional Edition.

    **Note:** You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.


## Query messages by time

You can query messages in all partitions by time. If you do not know the message location, but know the time range in which messages are sent, you can specify a time point in the time range to query messages that are sent near the time point.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance from which you want to query messages is located.
3.  In the left-side navigation pane, click **Message Query**.
4.  On the **Message Query** page, select the instance. Then, click the **Query by Time** tab.
5.  Enter the topic name, select a partition, specify a time point, and then click **Search**.

    On the **Query by Time** tab, the query results are displayed. The messages that are displayed are determined by the partition that you specify.

    -   If you select **All**, the messages in all the partitions are displayed. However, if the total number of messages exceeds the limit that is determined by the instance edition, messages are randomly displayed.

        ![querybytimeall](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53127.png)

    -   If you specify a partition, the messages in the specified partition are displayed.

        ![querybytimespecified](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4450549951/p53128.png)

    |Parameter|Description|
    |---------|-----------|
    |Partitions|The ID of the partition for the topic to which the message is sent.     -   If you specify a partition, the messages in the specified partition are displayed.
    -   If you select All, messages in all partitions of the instance are displayed or are randomly displayed. |
    |Offset|The consumer offset of the message.|
    |TimeStamp|The timestamp of the producer when the message is sent or the value of the `timestamp` field that you specify in the `producer record`. **Note:**

    -   If you set the timestamp field, the specified value is displayed.
    -   If you do not set the timestamp field, the system time when the message is sent is used.
    -   A value in the format of 1970/x/x x:x:x indicates that the timestamp field is set to 0 or an invalid value.
    -   You cannot set the timestamp field on clients of Apache Kafka version 0.9 and earlier. |

6.  Optional. Click **Message Details** in the **Actions** column.

    |Parameter|Description|
    |---------|-----------|
    |Key|The key of the queried message. The key is forcibly converted to a string.|
    |Value|The content of the queried message. The content is forcibly converted to a string.|

7.  Click **Download Message** next to the Key or Value parameter to download the message. This operation is supported only for instances of Professional Edition.

    **Note:** You can download up to 10 MB of messages at a time. If the total size of the queried messages exceeds 10 MB, only the first 10 MB of message content can be downloaded.


