---
keyword: [kafka, logstash, input, vpc]
---

# Connect to Logstash as an input

Message Queue for Apache Kafka can be connected to Logstash as an input. This topic describes how to use Logstash to consume messages in Message Queue for Apache Kafka in a virtual private cloud \(VPC\) environment.

Ensure that the following actions are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).
-   Logstash is downloaded and installed. For more information, see [Installing Logstash](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Logstash establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instance Details** page, select the instance that is to be connected to Logstash as an input.

4.  In the **Basic Information** section, obtain the endpoint of the instance.

    ![basic info](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0207892951/p128191.png)

    **Note:** For information about the differences between endpoints, see [Comparison between endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).


## Step 2: Create a topic

Perform the following operations to create a topic for storing messages.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Topics**.

2.  On the **Topics** page, click **Create Topic**.

3.  In the **Create Topic** dialog box, enter the topic information and click **Create**.

    ![logstash_2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0207892951/p103888.png)


## Step 3: Send messages

Perform the following operations to send messages to the topic you created.

1.  On the **Topics** page of the Message Queue for Apache Kafka console, find the topic you created and click **Send Message** in the **Actions** column.

2.  In the **Send Message** dialog box, enter the message information and click **Send**.

    ![logstash_4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3028082951/p103896.png)


## Step 4: Create a consumer group

Perform the following operations to create a consumer group for Logstash.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Consumer Groups**.

2.  On the **Consumer Groups** page, click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, enter the consumer group information and click **Create**.

    ![logstash_3](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3028082951/p103892.png)


## Step 5: Use Logstash to consume messages

Start Logstash on the server where Logstash has been installed, and consume messages from the created topic.

1.  Run the cd command to switch to the bin directory of Logstash.

2.  Create the input.conf configuration file.

    1.  Run the `vim input.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        input {
         kafka {
             bootstrap_servers => "192.168.XXX.XXX:9092,192.168.XXX.XXX:9092,192.168.XXX.XXX:9092"
             group_id => "logstash_group"
             topics => ["logstash_test"]
             consumer_threads => 12
             auto_offset_reset => "earliest"
         }
        }
        output {
         stdout{codec=>rubydebug}
        }
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |bootstrap\_servers|Message Queue for Apache Kafka supports the following VPC endpoints:         -   Default endpoint
        -   Simple Authentication and Security Layer \(SASL\) endpoint
|192.168.XXX.XXX:9092,192.168.XXX.XXX:9092,192.168.XXX.XXX:9092|
        |group\_id|The name of the consumer group.|logstash\_group|
        |topics|The name of the topic.|logstash\_test|
        |consumer\_threads|The number of consumer threads. We recommend that you set it to the number of partitions of the topic.|12|
        |auto\_offset\_reset|The reset offset. Valid values:         -   earliest: The earliest message is read.
        -   latest: The latest message is read.
|earliest|

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

3.  Run the following command to consume messages:

    ```
    ./logstash -f input.conf
    ```

    The following output is obtained.

    ![logstash_5](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3028082951/p103936.png)


## References

For more information about parameter settings, see [Kafka input plugin](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-kafka.html).

