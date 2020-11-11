---
keyword: [kafka, filebeat, input, vpc]
---

# Connect to Filebeat as an input

Message Queue for Apache Kafka can be connected to Filebeat as an input. This topic describes how to use Filebeat to consume messages in Message Queue for Apache Kafka in a virtual private cloud \(VPC\) environment.

Ensure that the following actions are completed:

-   A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).
-   Filebeat is downloaded and installed. For more information, see [Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html).
-   Java Development Kit \(JDK\) 8 is downloaded and installed. For more information, see [Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html).

## Step 1: Obtain an endpoint

Filebeat establishes a connection to Message Queue for Apache Kafka by using a Message Queue for Apache Kafka endpoint.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/).

2.  In the left-side navigation pane, click **Instances**.

3.  On the **Instance Details** page, select the instance to be connected to Filebeat as an input.

4.  In the **Basic Information** section, obtain the endpoint of the instance.

    ![basic info](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0207892951/p128191.png)

    **Note:** For information about the differences between endpoints, see [Comparison between endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).


## Step 2: Create a topic

Perform the following operations to create a topic for storing messages.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Topics**.

2.  On the **Topics** page, click **Create Topic**.

3.  In the **Create Topic** dialog box, enter the topic information and click **Create**.

    ![create_topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8228082951/p106204.png)


## Step 3: Send messages

Perform the following operations to send messages to the topic you created.

1.  On the **Topics** page of the Message Queue for Apache Kafka console, find the topic you created and click **Send Message** in the **Actions** column.

2.  In the **Send Message** dialog box, enter the message information and click **Send**.

    ![send_msg](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8228082951/p106203.png)


## Step 4: Create a consumer group

Perform the following operations to create a consumer group for Filebeat.

1.  In the left-side navigation pane of the Message Queue for Apache Kafka console, click **Consumer Groups**.

2.  On the **Consumer Groups** page, click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, enter the consumer group information and click **Create**.

    ![create_cg](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8228082951/p106205.png)


## Step 5: Use Filebeat to consume messages

Start Filebeat on the server where Filebeat has been installed to consume messages from the created topic.

1.  Run the cd command to switch to the installation directory of Filebeat:

2.  Create the input.yml configuration file.

    1.  Run the `vim input.conf` command to create an empty configuration file.

    2.  Press the i key to go to the insert mode.

    3.  Enter the following content:

        ```
        filebeat.inputs:
        - type: kafka
          hosts:
            - 192.168.XX.XX:9092
            - 192.168.XX.XX:9092
            - 192.168.XX.XX:9092
          topics: ["filebeat_test"]
          group_id: "filebeat_group"
        
        output.console:
          pretty: true
        ```

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |type|The input type of Filebeat.|kafka|
        |hosts|Message Queue for Apache Kafka supports the following VPC endpoints:         -   Default endpoint
        -   Simple Authentication and Security Layer \(SASL\) endpoint
|        ```
- 192.168.XX.XX:9092
- 192.168.XX.XX:9092
- 192.168.XX.XX:9092
        ``` |
        |topics|The name of the topic.|filebeat\_test|
        |group\_id|The name of the consumer group.|filebeat\_group|

        For more information about parameter settings, see [Kafka input plugin](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html).

    4.  Press the Esc key to return to the command line mode.

    5.  Press the : key to enter the bottom line mode. Type wq, and then press Enter to save the file and exit.

3.  Run the following command to consume messages:

    ```
    ./filebeat -c ./input.yml
    ```

    ![result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8228082951/p106207.png)


