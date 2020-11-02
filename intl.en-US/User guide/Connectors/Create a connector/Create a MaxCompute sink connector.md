---
keyword: [kafka, connector, maxcompute]
---

# Create a MaxCompute sink connector

This topic describes how to create a MaxCompute sink connector to export data from topics in a Message Queue for Apache Kafka instance to tables in MaxCompute.

Before you create a MaxCompute sink connector, ensure that you have completed the following operations:

1.  Enable the Connector feature for the Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the Connector feature.md).
2.  Create topics in Message Queue for Apache Kafka. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    A topic named maxcompute-test-input is used as an example.

3.  Create tables on the MaxCompute client. For more information, see [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md).

    A table named test\_kafka is used as an example. The following information shows the SQL statement for creating the test\_kafka table:

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,`partition` BIGINT,`offset` BIGINT,key STRING,value STRING,dt DATETIME) STORED AS ALIORC TBLPROPERTIES ('comment'='');
    ```


## Procedure

Perform the following steps to export data from a topic in the Message Queue for Apache Kafka instance to a table in MaxCompute by using a MaxCompute sink connector:

1.  Optional. Create topics and consumer groups that the MaxCompute sink connector requires.

    If you do not want to customize the names of the topics and consumer groups, skip this step and select Automatically in the next step.

    **Note:** Topics required by some MaxCompute sink connectors must use a local storage engine. The Message Queue for Apache Kafka instance running major version 0.10.2 does not allow you to manually create topics by using a local storage engine. Instead, the topics must be automatically created.

    1.  [Create topics that a MaxCompute sink connector requires](#section_jvw_8cp_twy)
    2.  [Create consumer groups that a MaxCompute sink connector requires](#section_xu7_scc_88s)
2.  Create and deploy a MaxCompute sink connector.
    1.  [Create a MaxCompute sink connector](#section_mjv_rqc_6ds)
    2.  [Deploy the MaxCompute sink connector](#section_444_q49_c46)
3.  Verify the result.
    1.  [Send messages](#section_idc_z6c_c33)
    2.  [View data in the table](#section_l1n_2qx_7kl)

## Create topics that a MaxCompute sink connector requires

In the Message Queue for Apache Kafka console, you can manually create five topics for a MaxCompute sink connector.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Topics**.

4.  On the **Topics** page, click **Create Topic**.

5.  In the **Create Topic** dialog box, set attributes for a topic and then click **Create**.

    |Topic|Description|
    |-----|-----------|
    |Task site topic|The topic that is used to store consumer offsets.    -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
    -   Partitions: the number of partitions in the topic. The value must be greater than 1.
    -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. The value must be compact. |
    |Task configuration topic|The topic that is used to store task configurations.    -   Topic: the name of the topic. We recommend that you start the name with connect-config.
    -   Partitions: the number of partitions in the topic. The value must be 1.
    -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. The value must be compact. |
    |Task status topic|The topic that is used to store task statuses.    -   Topic: the name of the topic. We recommend that you start the name with connect-status.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. The value must be compact. |
    |Dead-letter queue topic|The topic that is used to store abnormal data of the connector framework. To reduce topics, this topic can be reused as the abnormal data topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage. |
    |Abnormal data topic|The topic that is used to store abnormal data of the sink connector. To reduce topics, this topic can be reused as the dead-letter queue topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage. |


## Create consumer groups that a MaxCompute sink connector requires

In the Message Queue for Apache Kafka console, you can manually create two consumer groups for a MaxCompute sink connector.

1.  In the left-side navigation pane, click **Consumer Groups**.

2.  On the **Consumer Groups** page, select an instance and then click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, set attributes for the consumer group and then click **Create**.

    |Consumer Group|Description|
    |--------------|-----------|
    |Connector task consumer group|The consumer group that is used by data synchronization tasks. The name of this consumer group must be in the connect-task name format.|
    |Connector consumer group|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create a MaxCompute sink connector

To synchronize data from Message Queue for Apache Kafka to MaxCompute, perform the following steps to create a MaxCompute sink connector:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  On the **Connector** page, click **Create Connector**.

4.  On the **Create Connector** page, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list, select **MaxCompute** from the **Dump To** drop-down list, and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Connector Name|The name of the connector. The value must comply with the following rules:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and dashes \(-\), but cannot start with a dash \(-\).
        -   The name must be unique within a Message Queue for Apache Kafka instance.
Data synchronization tasks of a connector must use the consumer group named connect-task name. If you have not manually created such a consumer group, the system automatically creates one.

|kafka-maxcompute-sink|
        |Task Type|The type of the data synchronization task in the connector. In this example, the data synchronization task synchronizes data from Message Queue for Apache Kafka to MaxCompute. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2ODPS|

    2.  In the **Configure Source Instance** step, enter a topic name in the **Data Source Topic** field, select a consumer offset from the **Consumer Offset** drop-down list, and then select **Automatically** or **Manually** for **Create Resource**. If you select Manually, you must enter the name of the manually created topic. Then, click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |VPC ID|The virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the VPC where the Message Queue for Apache Kafka instance resides. You do not need to set the parameter.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|The vSwitch where the data synchronization task runs. The vSwitch must be in the same VPC as the Message Queue for Apache Kafka instance. The default value is the vSwitch that you specified for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |Data Source Topic|The topic whose data needs to be synchronized.|maxcompute-test-input|
        |Consumer Offset|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |Connector consumer group|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-maxcompute-sink|
        |Task site Topic|The topic that is used to store consumer offsets.        -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. The value must be greater than 1.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-offset-kafka-maxcompute-sink|
        |Task configuration Topic|The topic that is used to store task configurations.        -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. The value must be 1.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-config-kafka-maxcompute-sink|
        |Task status Topic|The topic that is used to store task statuses.        -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-status-kafka-maxcompute-sink|
        |Dead letter queue Topic|The topic that is used to store abnormal data of the connector framework. To reduce topics, this topic can be reused as the abnormal data topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage.
|connect-error-kafka-maxcompute-sink|
        |Abnormal Data Topic|The topic that is used to store abnormal data of the sink connector. To reduce topics, this topic can be reused as the dead-letter queue topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage.
|connect-error-kafka-maxcompute-sink|

    3.  In the **Configure Destination Instance** step, set attributes for the MaxCompute instance and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |MaxCompute Endpoint|The endpoint of MaxCompute. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).         -   VPC Endpoint: recommended for low latency. The VPC endpoint can be used when the Message Queue for Apache Kafka instance and the MaxCompute instance are in the same region.
        -   Public Endpoint: not recommended due to high latency. The public endpoint can be used when the Message Queue for Apache Kafka instance and the MaxCompute instance are in different regions. To use the public endpoint, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
        |MaxCompute Workspace|The name of the MaxCompute workspace.|connector\_test|
        |MaxCompute Table|The name of the MaxCompute table.|test\_kafka|
        |Alibaba Cloud Account ID|The AccessKey ID of the Alibaba Cloud account that is used to log on to MaxCompute.|LTAI4F\*\*\*|
        |Alibaba Cloud Account Key|The AccessKey secret of the Alibaba Cloud account that is used to log on to MaxCompute.|wvDxjjR\*\*\*|

5.  In the **Preview/Create** step, confirm the configurations of the connector and then click **Submit**.

    After you submit the configurations, refresh the **Connector** page. The connector that you created appears on the Connector page.


## Deploy the MaxCompute sink connector

After you create the MaxCompute sink connector, you must deploy it to synchronize data from Message Queue for Apache Kafka to MaxCompute.

1.  On the **Connector** page, find the MaxCompute sink connector that you created and then click **Deploy** in the **Actions** column.

    After you deploy the MaxCompute sink connector, it enters the Running state.


## Send messages

After you deploy the MaxCompute sink connector, you can send messages to topics in Message Queue for Apache Kafka to test whether the data can be synchronized to MaxCompute.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, select an instance, find **maxcompute-test-input**, and then click **Send Message** in the **Actions** column.

3.  In the **Send Message** dialog box, send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## View data in the table

After you send a message to a topic in Message Queue for Apache Kafka, you can view the data in the table on the MaxCompute client to check whether the message is received.

1.  [Install and configure the odpscmd client](/intl.en-US/Prepare/Install and configure the odpscmd client.md).

2.  Run the following command to view data in the test\_kafka table:

    ```
    SELECT COUNT(*) FROM test_kafka;
    ```

    The response displays the sent test message.


