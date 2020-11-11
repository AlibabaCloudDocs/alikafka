---
keyword: [, , ]
---

# Connect to Filebeat as an output

Message Queue for Apache Kafka can be connected to Filebeat as an output. This topic describes how to use Filebeat to send messages to Message Queue for Apache Kafka in a virtual private cloud \(VPC\) environment.

Ensure that the following actions are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).
-   Filebeat is downloaded and installed. For more information, see [Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Filebeat establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instance Details** page, select the instance to be connected to Filebeat as an output.

4.  In the **Basic Information** section, obtain the endpoint of the instance.

    ![basic info](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0207892951/p128191.png)

    **Note:** For information about the differences between endpoints, see [Comparison between endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).


## Step 2: Create a topic

Perform the following operations to create a topic for storing messages.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Topics**.

2.  On the **Topics** page, click **Create Topic**.

3.  In the **Create Topic** dialog box, enter the topic information and click **Create**.

    ![create_topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8228082951/p106204.png)


## Step 3: Use Filebeat to send a message

Start Filebeat on the server where Filebeat has been installed to send a message to the topic you created.

1.  Run the cd command to switch to the installation directory of Filebeat.

2.  Create the output.conf configuration file.

    1.  Run the `vim output.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        filebeat.inputs:
        - type: stdin
        
        output.kafka:
          hosts: ["192.XX.XX.XX:9092", "192.XX.XX.XX:9092", "192.XX.XX.XX:9092"]
        
          topic: 'filebeat_test'
        
          required_acks: 1
          compression: none
          max_message_bytes: 1000000
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |hosts|Message Queue for Apache Kafka supports the following VPC endpoints:         -   Default endpoint
        -   Simple Authentication and Security Layer \(SASL\) endpoint
|192.168.XXX.XXX:9092,192.168.XXX.XXX:9092,192.168.XXX.XXX:9092|
        |topic|The name of the topic.|filebeat\_test|
        |required\_acks|The reliability level of ACK. Valid values:         -   0: no response
        -   1: waits for local commit
        -   -1: waits for all replicas to commit
Default value: 1.|1|
        |compression|The data compression codec. Default value: gzip. Valid values:         -   none: none
        -   snappy: used to compress and decompress the SDK for C++.
        -   lz4: the lossless data compression algorithm focusing on compression and decompression speed.
        -   gzip: the file compression program for GNU free software.
|none|
        |max\_message\_bytes|The maximum size of a message. Unit: byte. Default value: 1000000. The value must be smaller than the maximum message size you have configured for Message Queue for Apache Kafka.|1000000|

        For more information about parameter settings, see [Kafka output plugin](https://www.elastic.co/guide/en/beats/filebeat/current/kafka-output.html).

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

3.  Send a message to the topic you created.

    1.  Run the `./filebeat -c ./output.yml` command.

    2.  Enter test and then press Enter.


## Step 4: View the partitions of the topic

Perform the following operations to view the message that was sent to the topic.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Topics**.

2.  On the **Topics** page, select the instance that is to be connected to Filebeat as an output, find the topic to which the message was sent, and then click **Partition Status** in the **Actions** column.

3.  On the **Partition Status** page, click **Refresh**.

    The following figure shows the partition ID and offset information of the message sent to the topic.

    ![topic_status](../images/p107774.png)


## Step 5: Query the message by offset

You can query the sent message based on its partition ID and offset information.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Message Query**.

2.  On the **Message Query** page, click the **Query by Offset** tab.

3.  Enter the topic to which the message was sent, select the partition ID for the message, enter the offset for the message, and then click **Search**.

    ![query_1](../images/p107775.png)

4.  Find the search result, and click **Message Details** in the **Actions** column.

    ![query_2](../images/p107776.png)


