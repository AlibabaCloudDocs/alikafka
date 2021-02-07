---
keyword: [kafka, filebeat, output, internet]
---

# Connect a Message Queue for Apache Kafka instance to Filebeat as an output

A Message Queue for Apache Kafka instance can be connected to Filebeat as an output. This topic shows you how to send messages to Message Queue for Apache Kafka over the Internet by using Filebeat.

Before you start this tutorial, make sure that the following operations are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from the Internet and VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Access from the Internet and VPC.md).
-   Filebeat is downloaded and installed. For more information, see [Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Filebeat establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, select the instance that you want to connect to Filebeat as an output.

4.  On the **Instance Details** page, obtain an endpoint of the instance in the **Basic Information** section.

    ![endpointzh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0695342161/p232431.png)

    **Note:** For information about the differences among endpoints, see [Comparison among endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).


## Step 2: Create a topic

Perform the following operations to create a topic for storing messages:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Topics**.

5.  On the **Topics** page, click **Create Topic**.

6.  In the **Create Topic** dialog box, enter the topic information and click **Create**.

    ![create topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0695342161/p232533.png)


## Step 3: Use Filebeat to send a message

Start Filebeat on the server where Filebeat is installed to send a message to the topic that you created.

1.  Run the cd command to switch to the installation directory of Filebeat.

2.  Run the following command to download the CA certificate file:

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert
    ```

3.  Create a configuration file named output.conf.

    1.  Run the `vim output.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        filebeat.inputs:
        - type: stdin
        
        output.kafka:
          hosts: ["121.XX.XX.XX:9093", "120.XX.XX.XX:9093", "120.XX.XX.XX:9093"]
          username: "alikafka_pre-cn-v641e1d***"
          password: "aeN3WLRoMPRXmAP2jvJuGk84Kuuo***"
        
          topic: 'filebeat_test'
          partition.round_robin:
            reachable_only: false
          ssl.certificate_authorities: ["/root/filebeat/filebeat-7.7.0-linux-x86_64/tasks/vpc_ssl/ca-cert"]
          ssl.verification_mode: none
        
          required_acks: 1
          compression: none
          max_message_bytes: 1000000
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |hosts|The public endpoint provided by Message Queue for Apache Kafka is the Secure Sockets Layer \(SSL\) endpoint.|121.XX.XX.XX:9093, 120.XX.XX.XX:9093, 120.XX.XX.XX:9093|
        |username|The username of your Message Queue for Apache Kafka instance of the Internet and VPC type.|alikafka\_pre-cn-v641e1d\*\*\*|
        |password|The password of your Message Queue for Apache Kafka instance of the Internet and VPC type.|aeN3WLRoMPRXmAP2jvJuGk84Kuuo\*\*\*|
        |topic|The name of the topic.|filebeat\_test|
        |reachable\_only|Specifies whether the message is sent only to available partitions. Valid values:         -   true: If the primary partition is unavailable, the output may be blocked.
        -   false: Even if the primary partition is unavailable, the output is not blocked.
|false|
        |ssl.certificate\_authorities|The path of the CA certificate file.|/root/filebeat/filebeat-7.7.0-linux-x86\_64/ca-cert|
        |ssl.verification\_mode|The verification mode.|none|
        |required\_acks|The reliability level of acknowledgments \(ACK\). Valid values:         -   0: no response
        -   1: waits for local commit
        -   -1: waits for all replicas to commit
Default value: 1.|1|
        |compression|The data compression codec. Default value: gzip. Valid values:         -   none: none
        -   snappy: the C ++ development package used for compression and decompression
        -   lz4: the lossless data compression algorithm focused on compression and decompression speed
        -   gzip: the file compression program for GNU free software
|none|
        |max\_message\_bytes|The maximum size of a message. Unit: bytes. Default value: 1000000. The value must be smaller than the maximum message size that you set for Message Queue for Apache Kafka.|1000000|

        For more information about parameter settings, see [Kafka output plugin](https://www.elastic.co/guide/en/beats/filebeat/current/kafka-output.html).

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

4.  Send a message to the topic that you created.

    1.  Run the `./filebeat -c ./output.yml` command.

    2.  Enter test and then press Enter.


## Step 4: View the partitions of the topic

Perform the following operations to view the message that was sent to the topic:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Topics**.

5.  On the **Topics** page, find the topic to which the message was sent, and click **Partition Status** in the **Actions** column.

6.  In the **Partition Status** dialog box, click ![image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0749862161/p240070.png).

    The following figure shows the partition ID and offset information of the message that was sent to the topic.

    ![topic_atatus](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4506342161/p232509.png)


## Step 5: Query the message by offset

You can query the sent message based on its partition ID and offset information.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Message Query**.

5.  On the **Message Query** page, click the **Query by Offset** tab.

6.  Enter the topic of the sent message in the **Enter a topic.** search box, select the partition ID of this message from the **Select a partition.** drop-down list, and then enter the offset of this message in the **Enter an offset.** search box. Then, click **Search**.

7.  On the right side of the query result, you can view the message details.


