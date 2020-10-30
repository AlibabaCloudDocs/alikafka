---
keyword: [kafka, connector, maxcompute]
---

# Create a MaxCompute sink connector

This topic describes how to create a MaxCompute sink connector to export data from Message Queue for Apache Kafka to MaxCompute.

Before you create a MaxCompute sink connector, ensure that you have completed the following operations:

1.  Purchase a Message Queue for Apache Kafka instance and deploy it. For more information, see [Connect Message Queue for Apache Kafka to a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).
2.  Enable the connector feature for the Message Queue for Apache Kafka instance. For more information, see [Enable the Connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
3.  Create a table named test\_kafka in the MaxCompute client by running the following statement:

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,`partition` BIGINT,`offset` BIGINT,key STRING,value STRING,dt DATETIME) STORED AS ALIORC TBLPROPERTIES ('comment'='');
    ```

    For more information, see [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md).


## Procedure

Complete the following steps to export data from a topic in Message Queue for Apache Kafka to MaxCompute by using a MaxCompute sink connector:

1.  Create the Message Queue for Apache Kafka resources that a MaxCompute sink connector requires.
    1.  [Create consumer groups](#section_98y_4nx_ntk)
    2.  [Create topics](#section_b0d_bsr_qy7)
2.  Create a MaxCompute sink connector.

    [Create a MaxCompute sink connector](#section_mjv_rqc_6ds)

3.  Verify the result.
    1.  [Send messages](#section_idc_z6c_c33)
    2.  [Query the table data](#section_l1n_2qx_7kl)

## Create consumer groups

Create consumer groups for performing data synchronization tasks by using a MaxCompute sink connector.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Consumer Groups**.

4.  On the **Consumer Groups** page, click **Create Consumer Group**.

5.  In the **Create Consumer Group** dialog box, create a consumer group.

    1.  In the **Consumer Group** field, enter connect-kafka-maxcompute-sink.

    2.  In the **Description** field, enter connect-kafka-maxcompute-sink.

    3.  Click **Create**.

6.  Create a connector consumer group by repeating Steps 4 and 5.

    |Consumer group type|Parameter|Description|Example|
    |-------------------|---------|-----------|-------|
    |Connector consumer group|Consumer Group|The name of the consumer group. We recommend that you start the name with connect-cluster.|connect-cluster-kafka-maxcompute-sink|

    The created consumer group appears on the **Consumer Groups** page.


## Create topics

Create topics for performing data synchronization tasks by using a MaxCompute sink connector.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, click **Create Topic**.

3.  In the **Create Topic** dialog box, create a data source topic.

    1.  In the **Topic** field, enter maxcompute-test-input.

    2.  In the **Description** field, enter maxcompute-test-input.

    3.  Click **Create**.

4.  Create the following topic by repeating Steps 2 and 3.

    |Topic type|Parameter|Description|Example|
    |----------|---------|-----------|-------|
    |Task site topic|Topic|The name of the topic. We recommend that you start the name with connect-offset.|connect-offset-maxcompute-sink|
    |Partitions|The number of partitions in the topic. The value must be greater than 1.|12|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Task configuration topic|Topic|The name of the topic. We recommend that you start the name with connect-config.|connect-config-maxcompute-sink|
    |Partitions|The number of partitions in the topic. The value must be 1.|1|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Task status topic|Topic|The name of the topic. We recommend that you start the name with connect-status.|connect-status-maxcompute-sink|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Dead-letter queue topic|Topic|The name of the topic.|maxcompute\_dead\_letter\_error|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value can be Local Storage or Cloud Storage.|Cloud Storage|
    |Abnormal data topic|Topic|The name of the topic.|maxcompute\_runtime\_error|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value can be Local Storage or Cloud Storage.|Cloud Storage|

    **Note:** You can combine the dead-letter queue topic and the abnormal data topic into one to store abnormal data.

    The created topics appear on the **Topics** page.


## Create a MaxCompute sink connector

After you create the consumer groups and topics for performing data synchronization tasks by using a MaxCompute sink connector, create a MaxCompute sink connector to export data from the topics in Message Queue for Apache Kafka to MaxCompute.

1.  On the **Connector** page, click **Create Connector**.

2.  On the **Create Connector** page, enter the connector information and click **Pre-Check and Create**.

    |Section|Parameter|Description|Example|
    |-------|---------|-----------|-------|
    |Task Information|Task Name|The name of the data synchronization task. The name must be unique within the instance. The synchronization tasks that are performed by using a connector use the consumer groups named in the connector-task name format. Therefore, before you create a connector, create the consumer groups named in the connector-task name format.|kafka-maxcompute-sink|
    |General Information|Task Type|The type of the data synchronization task. For more information about the task types that Message Queue for Apache Kafka supports, see [Types of connectors](/intl.en-US/User guide/Connectors/Connectors overview.md).|KAFKA2ODPS|
    |User VPC|The virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the VPC where the Message Queue for Apache Kafka instance is deployed, and you do not need to enter the value.|vpc-bp1xpdnd3l\*\*\*|
    |VSwitch|The VSwitch where the data synchronization task runs. The VSwitch must be on the same VPC as the Message Queue for Apache Kafka instance. The default value is the VPC where the Message Queue for Apache Kafka instance is deployed in zone H.|vsw-bp1d2jgg81\*\*\*|
    |Source Instance Information|Data Source Topic|The topic whose data needs to be synchronized.|maxcompute-test-input|
    |Consumer Offset|The offset where consumption starts. Valid values:     -   latest: Consumption starts from the latest offset.
    -   earliest: Consumption starts from the initial offset.
|latest|
    |Connector Consumer Group|The consumer group that is used for data synchronization.|connect-cluster-kafka-maxcompute-sink|
    |Task Site Topic|The topic that is used to store consumer offsets.|connect-offset-maxcompute-sink|
    |Task Configuration Topic|The topic that is used to store task configurations.|connect-config-maxcompute-sink|
    |Task Status Topic|The topic that is used to store task statuses.|connect-status-maxcompute-sink|
    |Dead-letter Queue Topic|The topic that is used to store the abnormal data of the connector framework.|maxcompute\_dead\_letter\_error|
    |Abnormal Data Topic|The topic that is used to store the abnormal data of the sink connector.|maxcompute\_runtime\_error|
    |Destination Instance Information|MaxCompute Endpoint|The endpoint of MaxCompute. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).     -   VPC Endpoint: recommended for low latency. The VPC endpoint can be used when the Message Queue for Apache Kafka instance and MaxCompute are in the same region.
    -   Public Endpoint: not recommended due to high latency. The Public Endpoint value can be used when the Message Queue for Apache Kafka instance and MaxCompute are in different regions. To use the Public Endpoint value, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
    |MaxCompute Workspace|The name of the MaxCompute workspace.|connector\_test|
    |MaxCompute Table|The name of the MaxCompute table.|test\_kafka|
    |Alibaba Cloud Account ID|The AccessKey ID of the Alibaba Cloud account to which the MaxCompute instance belongs.|LTAI4F\*\*\*|
    |Alibaba Cloud Account Key|The AccessKey secret of the Alibaba Cloud account to which the MaxCompute instance belongs.|wvDxjjR\*\*\*|

    The created MaxCompute sink connector appears on the **Connector** page.


## Send messages

After you create the MaxCompute sink connector to connect Message Queue for Apache Kafka and MaxCompute, send a test message to the data source topic in Message Queue for Apache Kafka.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, find **maxcompute-test-input** and click **Send Message** in the **Actions** column.

3.  In the **Send Message** dialog box, send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## Query the table data

After you send the message to the data source topic in Message Queue for Apache Kafka, view the table data in the MaxCompute client to check whether the message is received.

1.  [Install and configure the odpscmd client](/intl.en-US/Prepare/Install and configure the odpscmd client.md).

2.  Run the following command to view the data in the test\_kafka table:

    ```
    SELECT COUNT(*) FROM test_kafka;
    ```

    The test message appears in the response.


-   [View the task configurations of a connector](/intl.en-US/User guide/Connectors/View the task configurations of a connector.md)
-   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md)
-   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md)
-   [Modify the connector description](/intl.en-US/User guide/Connectors/Modify connector descriptions.md)

