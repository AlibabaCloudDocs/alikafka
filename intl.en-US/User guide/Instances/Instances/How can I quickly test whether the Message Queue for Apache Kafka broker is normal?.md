# How can I quickly test whether the Message Queue for Apache Kafka broker is normal?

After creating and deploying a Message Queue for Apache Kafka instance, you can directly send messages through the Message Queue for Apache Kafka console to quickly test whether the broker is normal.

You have created and deployed a Message Queue for Apache Kafkainstance, and the instance is in the **Running** state.

## Procedure

Test the Message Queue for Apache Kafka broker as follows:

1.  [Create a topic](#section_jax_bs9_o5x)
2.  [Send a message](#section_ldk_ge6_y1v)
3.  [View the partition status](#section_cyx_ddi_5vi)
4.  [Query a message by offset](#section_tar_w7j_afd)

You can repeat Steps 2 through 4 multiple times. If all repeated steps are successful, the broker works properly.

**Note:** If the broker works properly but messages cannot be sent, we recommend that you check the caller, such as the native client or an ecosystem component.

## Create a topic

Create a topic for receiving messages.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Topics**.

3.  On the top of the **Topics** page, click the target instance, and then click **Create Topic**.

    ![Create a topic](../images/p87665.png)

4.  In the **Create Topic**dialog box, set topic properties and then click **Create**.

    The fields are described as follows:

    -   **Topic**: the name of a topic, for example, demo.
    -   **Tags**: the tag of the topic, for example, demo.
    -   **Instance**: the ID of the instance, for example, alikafka\_pre-cn-\*\*\*.
    -   **Description**: the description about the topic, for example, demo.
    -   **Partitions**: the number of partitions for the topic, for example, 12.
    ![createtopic](../images/p87632.png)


## Send a message

Send a message to the specified partition of the created topic.

1.  On the **Topics** page, find the created topic. In the **Actions** column, click **Send Message**.

    ![sendmessage](../images/p87667.png)

2.  In the **Send Message**dialog box, set a partition and message properties, and then click **Send**.

    The fields are described as follows:

    -   **Partition**: the ID of a partition, for example, 0.
    -   **Message Key**: the key of the message to be sent, for example, demo.
    -   **Message Value**: the value of the message, for example, \{"key": "test"\}.
    ![Configure message sending](../images/p87671.png)


## View the partition status

After sending a message to the specified partition, you can view the partition status.

1.  On the **Topics** page, find the topic to which the message is sent. In the **Actions** column, click **Partition Status**.

    ![sendmessage](../images/p87667.png)

2.  In the **Partition Status** dialog box, click **Refresh**.

    ![update](../images/p87686.png)


## Query a message by offset

Query a message based on the partition ID and offset.

1.  In the left-side navigation pane, click **Message Query**.

2.  On the **Message Query** page, click the target instance and click the **Query by Offset** tab.

    1.  In the **Enter a topic.** field, enter a topic.

    2.  From the **Select a partition.** drop-down list, select the ID of the partition to which the message is sent.

    3.  In the **Enter an offset.** field, enter an offset.

    4.  Click **Search**.

    ![query message](../images/p87737.png)


