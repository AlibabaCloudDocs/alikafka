# How can I fast test whether the Message Queue for Apache Kafka broker is normal?

After you purchase and deploy a Message Queue for Apache Kafka instance, you can directly send messages in the Message Queue for Apache Kafka console to fast test whether the broker is normal.

A Message Queue for Apache Kafka instance is purchased and deployed, and it is in the **Running** state.

## Procedure

To test the Message Queue for Apache Kafka broker, perform the following steps:

1.  [Create a topic](#section_jax_bs9_o5x)
2.  [Send messages](#section_ldk_ge6_y1v)
3.  [View the partition status](#section_cyx_ddi_5vi)
4.  [Query a message by offset](#section_tar_w7j_afd)

You can repeat Steps 2 through 4 multiple times. If all repeated steps are successful, the broker works properly.

**Note:** If the broker works properly but messages cannot be sent, we recommend that you check the caller, such as the native client or an ecosystem component.

## Create a topic

Create a topic for receiving messages.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Topics**.

3.  On the top of the **Topics** page, click the instance on which you want to create the topic, and then click **Create Topic**.

    ![create topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0612597161/p87665.png)

4.  In the **Create Topic** dialog box, set topic properties and then click **Create**.

    ![createtopic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6647967161/p87632.png)

    Descriptions of the parameters are provided:

    -   **Topic**: the name of a topic, for example, demo.
    -   **Tags**: the tag of the topic, for example, demo.
    -   **Instance**: the ID of the instance, for example, alikafka\_pre-cn-\*\*\*.
    -   **Description**: the description about the topic, for example, demo.
    -   **Partitions**: the number of partitions for the topic, for example, 12.

## Send messages

Send a message to the specified partition of the created topic.

1.  On the **Topics** page, find the created topic and click **Send Message** in the **Actions** column.

    ![sendmessage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1612597161/p87667.png)

2.  In the **Send Message** dialog box, set a partition and message properties, and then click **Send**.

    ![sendmessagesetting](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6647967161/p87671.png)

    Descriptions of the parameters are provided:

    -   **Partition**: the ID of a partition, for example, 0.
    -   **Message Key**: the key of the message to be sent, for example, demo.
    -   **Message Value**: the value of the message, for example, \{"key": "test"\}.

## View the partition status

After you send a message to the specified partition, you can view the partition status.

1.  On the **Topics** page, find the topic to which the message is sent and click **Partition Status** in the **Actions** column.

    ![sendmessage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1612597161/p87667.png)

2.  In the **Partition Status** dialog box, click **Refresh**.

    ![update](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6647967161/p87686.png)


## Query a message by offset

Query a message based on the partition ID and offset.

1.  In the left-side navigation pane, click **Message Query**.

2.  On the **Message Query** page, click the instance and then click the **Query by Offset** tab.

    1.  In the **Enter a topic.** field, enter a topic.

    2.  From the **Select a partition.** drop-down list, select the ID of the partition to which the message is sent.

    3.  In the **Enter an offset.** field, enter an offset.

    4.  Click the **Search** tab.

    ![query message](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1612597161/p87737.png)


