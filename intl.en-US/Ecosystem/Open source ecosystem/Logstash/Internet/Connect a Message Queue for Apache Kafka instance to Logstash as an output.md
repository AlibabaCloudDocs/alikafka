---
keyword: [kafka, logstash, output, internet]
---

# Connect a Message Queue for Apache Kafka instance to Logstash as an output

A Message Queue for Apache Kafka instance can be connected to Logstash as an output. This topic shows you how to send messages to Message Queue for Apache Kafka over the Internet by using Logstash.

Before you start this tutorial, make sure that the following operations are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from the Internet and VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Access from the Internet and VPC.md).
-   Logstash is downloaded and installed. For more information, see [Download Logstash](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Logstash establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, select the instance that you want to connect to Logstash as an output.

4.  On the **Instance Details** page, obtain an endpoint of the instance in the **Basic Information** section.

    ![endpointzh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0695342161/p232431.png)

    **Note:** For information about the differences among endpoints, see [Comparison among endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).

5.  In the **Security Configuration** section, obtain the username and password of the instance.


## Step 2: Create a topic

Perform the following operations to create a topic for storing messages:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Topics**.

5.  On the **Topics** page, click **Create Topic**.

6.  In the **Create Topic** dialog box, enter the topic information and click **Create**.

    ![create topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0695342161/p232533.png)


## Step 3: Use Logstash to send a message

Start Logstash on the server where Logstash is installed, and send a message to the topic that you created.

1.  Run the cd command to switch to the bin directory of Logstash.

2.  Run the following command to download the kafka.client.truststore.jks certificate file:

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-log-stash-demo/vpc-ssl/kafka.client.truststore.jks
    ```

3.  Create a configuration file named jaas.conf.

    1.  Run the `vim jaas.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          username="XXX"
          password="XXX";
        };
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |username|The username of your Message Queue for Apache Kafka instance of the Internet and VPC type.|alikafka\_pre-cn-v0h1\*\*\*|
        |password|The password of your Message Queue for Apache Kafka instance of the Internet and VPC type.|GQiSmqbQVe3b9hdKLDcIlkrBK6\*\*\*|

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

4.  Create a configuration file named output.conf.

    1.  Run the `vim output.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        input {
            stdin{}
        }
        
        output {
           kafka {
                bootstrap_servers => "121.40.XXX.XXX:9093,120.26.XXX.XXX:9093,120.26.XXX.XXX:9093"
                topic_id => "logstash_test"
                security_protocol => "SASL_SSL"
                sasl_mechanism => "PLAIN"
                jaas_path => "/home/logstash-7.6.2/bin/jaas.conf"
                ssl_truststore_password => "KafkaOnsClient"
                ssl_truststore_location => "/home/logstash-7.6.2/bin/kafka.client.truststore.jks"
                ssl_endpoint_identification_algorithm => ""
            }
        }
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |bootstrap\_servers|The public endpoint provided by Message Queue for Apache Kafka is the Secure Sockets Layer \(SSL\) endpoint.|121.XX.XX.XX:9093,120.XX.XX.XX:9093,120.XX.XX.XX:9093|
        |topic\_id|The name of the topic.|logstash\_test|
        |security\_protocol|The security protocol. Default value: SASL\_SSL. You do not need to change this value.|SASL\_SSL|
        |sasl\_mechanism|The security authentication mechanism. Default value: PLAIN. You do not need to change this value.|PLAIN|
        |jaas\_path|The path of the jaas.conf configuration file.|/home/logstash-7.6.2/bin/jaas.conf|
        |ssl\_truststore\_password|The password of the kafka.client.truststore.jks certificate. Default value: KafkaOnsClient. You do not need to change this value.|KafkaOnsClient|
        |ssl\_truststore\_location|The path of the kafka.client.truststore.jks certificate.|/home/logstash-7.6.2/bin/kafka.client.truststore.jks|
        |ssl\_endpoint\_identification\_algorithm|The identification algorithm of the SSL endpoint. This parameter is required for Logstash V6.x and later.|Null|

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

5.  Send a message to the topic that you created.

    1.  Run the `./logstash -f output.conf` command.

    2.  Enter test and then press Enter.

        ![output_result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4506342161/p106190.png)


## Step 4: View the partitions of the topic

Perform the following operations to view the message that was sent to the topic:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instances** page, click the name of your instance.

4.  In the left-side navigation pane, click **Topics**.

5.  On the **Topics** page, find the topic to which the message was sent, and click **Partition Status** in the **Actions** column.

6.  In the **Partition Status** dialog box, click ![image](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3759862161/p240072.png).

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


## References

For more information about parameter settings, see [Kafka output plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-kafka.html).

