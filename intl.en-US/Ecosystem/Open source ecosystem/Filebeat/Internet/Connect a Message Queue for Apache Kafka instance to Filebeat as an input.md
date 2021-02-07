---
keyword: [kafka, filebeat, input, internet]
---

# Connect a Message Queue for Apache Kafka instance to Filebeat as an input

A Message Queue for Apache Kafka instance can be connected to Filebeat as an input. This topic shows you how to consume messages in Message Queue for Apache Kafka over the Internet by using Filebeat.

Before you start this tutorial, make sure that the following operations are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from the Internet and VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Access from the Internet and VPC.md).
-   Filebeat is downloaded and installed. For more information, see [Download Filebeat](https://www.elastic.co/cn/downloads/beats/filebeat).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Filebeat establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, select the instance that you want to connect to Filebeat as an input.

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


## Step 3: Send messages

Perform the following operations to send messages to the topic that you created:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Topics**.

5.  On the **Topics** page, find the created topic and click **Send Message** in the **Actions** column.

6.  In the **Send Message** dialog box, enter the message information and click **Send**.

    ![send](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0695342161/p232539.png)


## Step 4: Create a consumer group

Perform the following operations to create a consumer group for Filebeat:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Consumer Groups**.

5.  On the **Consumer Groups** page, click **Create Consumer Group**.

6.  In the **Create Consumer Group** dialog box, enter the consumer group information and click **Create**.

    ![filebeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6116342161/p232559.png)


## Step 5: Use Filebeat to consume messages

Start Filebeat on the server where Filebeat is installed to consume messages from the created topic.

1.  Run the cd command to switch to the installation directory of Filebeat:

2.  Run the following command to download the CA certificate file:

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert
    ```

3.  Create a configuration file named input.yml.

    1.  Run the `vim input.yml` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        filebeat.inputs:
        - type: kafka
          hosts:
            - 121.XX.XX.XX:9093
            - 120.XX.XX.XX:9093
            - 120.XX.XX.XX:9093
          username: "alikafka_pre-cn-v641e1dt***"
          password: "aeN3WLRoMPRXmAP2jvJuGk84Kuuo***"
          topics: ["filebeat_test"]
          group_id: "filebeat_group"
          ssl.certificate_authorities: ["/root/filebeat/filebeat-7.7.0-linux-x86_64/ca-cert"]
          ssl.verification_mode: none
        
        output.console:
          pretty: true
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |hosts|The public endpoint provided by Message Queue for Apache Kafka is the Secure Sockets Layer \(SSL\) endpoint.|        ```
- 121.XX.XX.XX:9093
- 120.XX.XX.XX:9093
- 120.XX.XX.XX:9093
        ``` |
        |username|The username of your Message Queue for Apache Kafka instance of the Internet and VPC type.|alikafka\_pre-cn-v641e1d\*\*\*|
        |password|The password of your Message Queue for Apache Kafka instance of the Internet and VPC type.|aeN3WLRoMPRXmAP2jvJuGk84Kuuo\*\*\*|
        |topics|The name of the topic.|filebeat\_test|
        |group\_id|The name of the consumer group.|filebeat\_group|
        |ssl.certificate\_authorities|The path of the CA certificate file.|/root/filebeat/filebeat-7.7.0-linux-x86\_64/ca-cert|
        |ssl.verification\_mode|The verification mode.|none|

        For more information about parameter settings, see [Kafka input plugin](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html).

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

4.  Run the following command to consume messages:

    ```
    ./filebeat -c ./input.yml
    ```

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6116342161/p107686.png)


