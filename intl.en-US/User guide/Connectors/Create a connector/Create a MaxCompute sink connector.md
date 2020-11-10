---
keyword: [kafka, connector, maxcompute]
---

# Create a MaxCompute sink connector

This topic describes how to create a MaxCompute sink connector to synchronize data from a source topic in a Message Queue for Apache Kafka instance to a table in MaxCompute.

The following operations are completed:

1.  Enable the connector feature for the Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
2.  Create a source topic in the Message Queue for Apache Kafka instance. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    For example, you can create a topic named maxcompute-test-input.

3.  Create a table on the MaxCompute client. For more information, see [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md).

    For example, you can execute the following SQL statement to create a table named test\_kafka:

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,`partition` BIGINT,`offset` BIGINT,key STRING,value STRING,dt DATETIME) STORED AS ALIORC TBLPROPERTIES ('comment'='');
    ```


## Procedure

To synchronize data from a source topic in a Message Queue for Apache Kafka instance to a table in MaxCompute by using a MaxCompute sink connector, perform the following steps:

1.  Optional. Create the topics and consumer groups that the MaxCompute sink connector requires.

    If you do not want to specify the names of the topics and consumer groups, skip this step and select Automatically in the next step.

    **Note:** Some topics require a local storage engine. If the major version of your Message Queue for Apache Kafka instance is 0.10.2, you cannot manually create topics that use a local storage engine. In major version 0.10.2, these topics must be automatically created.

    1.  [Create topics \(Optional\)](#section_jvw_8cp_twy)
    2.  [Create consumer groups \(Optional\)](#section_xu7_scc_88s)
2.  Create and deploy a MaxCompute sink connector.
    1.  [Create a MaxCompute sink connector](#section_mjv_rqc_6ds)
    2.  [Deploy the MaxCompute sink connector](#section_444_q49_c46)
3.  Verify the result.
    1.  [Send messages](#section_idc_z6c_c33)
    2.  [View data in the MaxCompute table](#section_l1n_2qx_7kl)

## Create topics \(Optional\)

In the Message Queue for Apache Kafka console, you can manually create the five topics that the MaxCompute sink connector requires.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Topics**.

4.  On the **Topics** page, click **Create Topic**.

5.  In the **Create Topic** dialog box, set the parameters as required and click **Create**.

    |Topic|Description|
    |-----|-----------|
    |Task site topic|The topic that is used to store consumer offsets.    -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
    -   Partitions: the number of partitions in the topic. The value must be greater than 1.
    -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact. |
    |Task configuration topic|The topic that is used to store task configurations.    -   Topic: the name of the topic. We recommend that you start the name with connect-config.
    -   Partitions: the number of partitions in the topic. Set the value to 1.
    -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact. |
    |Task status topic|The topic that is used to store task statuses.    -   Topic: the name of the topic. We recommend that you start the name with connect-status.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact. |
    |Dead-letter queue topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, you can use a dead-letter queue topic or an abnormal data topic to store all abnormal data.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |
    |Abnormal data topic|The topic that is used to store the abnormal data of the sink connector. To save topic resources, you can use an abnormal data topic or a dead-letter queue topic to store all abnormal data.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |


## Create consumer groups \(Optional\)

In the Message Queue for Apache Kafka console, you can manually create the two consumer groups that the MaxCompute sink connector requires.

1.  In the left-side navigation pane, click **Consumer Groups**.

2.  On the **Consumer Groups** page, select the instance and click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, set the parameters as required and click **Create**.

    |Consumer group|Description|
    |--------------|-----------|
    |Connector task consumer group|The name of the consumer group that is used by data synchronization tasks. The name of this consumer group must be in the connect-task name format.|
    |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create a MaxCompute sink connector

To create a MaxCompute sink connector that is used to synchronize data from Message Queue for Apache Kafka to MaxCompute, perform the following steps:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  On the **Connector** page, click **Create Connector**.

4.  In the **Create Connector** panel, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list, select **MaxCompute** from the **Dump To** drop-down list, and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Connector Name|The name of the connector. The value must comply with the following rules:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The name must be unique in a Message Queue for Apache Kafka instance.
The data synchronization tasks of a connector must use a consumer group that is named in the format of connect-task name. If you have not manually created such a consumer group, the system automatically creates a consumer group for you.

|kafka-maxcompute-sink|
        |Task Type|The type of the data synchronization task of the connector. A MaxCompute sink connector runs data synchronization tasks to synchronize data from Message Queue for Apache Kafka to MaxCompute. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2ODPS|

    2.  In the **Configure Source Instance** step, enter a topic name in the **Data Source Topic** field, select a consumer offset from the **Consumer Offset** drop-down list, set the **Create Resource** parameter to **Automatically** or **Manually**, and then click **Next**. If you select Manually, enter the name of each manually created topic.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |VPC ID|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is located. You can retain the default value.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch|The ID of the vSwitch based on which the data synchronization task runs. The vSwitch must be in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you specify for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |Data Source Topic|The name of the topic from which data is to be synchronized.|maxcompute-test-input|
        |Consumer Offset|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-maxcompute-sink|
        |Task site Topic|The topic that is used to store consumer offsets.        -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. The value must be greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-offset-kafka-maxcompute-sink|
        |Task configuration Topic|The topic that is used to store task configurations.        -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the value to 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-config-kafka-maxcompute-sink|
        |Task status Topic|The topic that is used to store task statuses.        -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-status-kafka-maxcompute-sink|
        |Dead letter queue Topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, you can use a dead-letter queue topic or an abnormal data topic to store all abnormal data.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-maxcompute-sink|
        |Abnormal Data Topic|The topic that is used to store the abnormal data of the sink connector. To save topic resources, you can use an abnormal data topic or a dead-letter queue topic to store all abnormal data.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-maxcompute-sink|

    3.  In the **Configure Destination Instance** step, set the parameters as required and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |MaxCompute Endpoint|The endpoint of MaxCompute. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).         -   VPC endpoint: The VPC endpoint can be used when the Message Queue for Apache Kafka instance and MaxCompute project are in the same region. We recommend that you use the VPC endpoint because it has lower latency.
        -   Public endpoint: The public endpoint can be used when the Message Queue for Apache Kafka instance and the MaxCompute project are in different regions. We recommend that you do not use the public endpoint because it has high latency. To use the public endpoint, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
        |MaxCompute Workspace|The name of the MaxCompute project to which you want to synchronize data.|connector\_test|
        |MaxCompute Table|The name of the MaxCompute table to which you want to synchronize data.|test\_kafka|
        |Alibaba Cloud account ID|The AccessKey ID of the Alibaba Cloud account that is used to log on to MaxCompute.|LTAI4F\*\*\*|
        |Alibaba Cloud Account Key|The AccessKey secret of the Alibaba Cloud account that is used to log on to MaxCompute.|wvDxjjR\*\*\*|

5.  In the **Preview/Create** step, confirm the configuration of the connector and click **Submit**.

    After you submit the configurations, refresh the **Connector** page. The connector that you create is displayed on the Connector page.


## Deploy the MaxCompute sink connector

After you create the MaxCompute sink connector, you must deploy it to synchronize data from Message Queue for Apache Kafka to MaxCompute.

1.  On the **Connector** page, find the MaxCompute sink connector that you create and click **Deploy** in the **Actions** column.

    After you deploy the MaxCompute sink connector, it enters the Running state.


## Send messages

After you deploy the MaxCompute sink connector, you can send messages to the source topic in Message Queue for Apache Kafka to test whether the data can be synchronized to MaxCompute.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, select the instance that contains the maxcompute-test-input topic, find the **maxcompute-test-input** topic, and then click **Send Message** in the **Actions** column.

3.  In the **Send Message** dialog box, send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## View data in the MaxCompute table

After you send a message to the source topic in Message Queue for Apache Kafka, you can view the data in the table on the MaxCompute client to check whether the message is received.

1.  [Install and configure the odpscmd client](/intl.en-US/Prepare/Install and configure the odpscmd client.md).

2.  Execute the following statement to view data in the test\_kafka table:

    ```
    SELECT COUNT(*) FROM test_kafka;
    ```

    The sent message is returned in the response.

    ![table_result](../images/p127744.png)


