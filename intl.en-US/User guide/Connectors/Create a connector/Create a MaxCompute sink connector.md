---
keyword: [kafka, connector, maxcompute]
---

# Create a MaxCompute sink connector

This topic describes how to create a MaxCompute sink connector to synchronize data from a data source topic of a Message Queue for Apache Kafka instance to a MaxCompute table.

Before you start, make sure that the following requirements are met:

1.  The connector feature for the Message Queue for Apache Kafka instance is enabled. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
2.  Data source topics for the Message Queue for Apache Kafka instance are created. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    A topic named maxcompute-test-input is created in this example.

3.  A MaxCompute table from the MaxCompute client is created. For more information, see [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md).

    A MaxCompute table named test\_kafka is created in a project named connector\_test in this example. You can run the following statement to create a MaxCompute table named test\_kafka:

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,partition BIGINT,offset BIGINT,key STRING,value STRING) PARTITIONED by (pt STRING);
    ```


## Procedure

To synchronize data from a data source topic of a Message Queue for Apache Kafka instance to a MaxCompute table by using a MaxCompute sink connector, perform the following steps:

1.  Grant Message Queue for Apache Kafka the permissions to access MaxCompute.
    -   [Create a RAM role](#section_e02_70i_3jg)
    -   [Add permissions](#section_rbd_pjy_5dh)
2.  Create topics and consumer groups that are required by the MaxCompute sink connector.

    If you do not want to specify the names of the topics and consumer groups, skip this step and select Automatically in the next step.

    **Note:** Some topics require a local storage engine. If the major version of your Message Queue for Apache Kafka instance is 0.10.2, you cannot manually create topics that use a local storage engine. In major version 0.10.2, these topics must be automatically created.

    1.  [Create topics that are required by the MaxCompute sink connector.](#section_jvw_8cp_twy)
    2.  [Create consumer groups that are required by the MaxCompute sink connector.](#section_xu7_scc_88s)
3.  [Create and deploy a MaxCompute sink connector.](#section_mjv_rqc_6ds)
4.  Verify the result.
    1.  [Send test messages](#section_idc_z6c_c33)
    2.  [View data in the MaxCompute table](#section_l1n_2qx_7kl)

## Create a RAM role

You cannot select Message Queue for Apache Kafka as the trusted service of a Resource Access Management \(RAM\) role. When you create the RAM role, select a supported service as the trusted service. Then, manually modify the trust policy of the created RAM role. To create a RAM role, perform the following steps:

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the **RAM Roles** page, click **Create RAM Role**.

4.  In the **Create RAM Role** pane, perform the following steps:

    1.  Set **Trusted entity type** to **Alibaba Cloud Service** and click **Next**.

    2.  Set **Role Type** to **Normal Service Role**. In the **RAM Role Name** field, enter AliyunKafkaMaxComputeUser1. From the **Select Trusted Service** drop-down list, select **Function Compute**. Then, click **OK**.

5.  On the **RAM Roles** page, find **AliyunKafkaMaxComputeUser1** and click **AliyunKafkaMaxComputeUser1**.

6.  On the **AliyunKafkaMaxComputeUser1** page, click the **Trust Policy Management** tab, and then click **Edit Trust Policy**.

7.  In the **Edit Trust Policy** pane, replace **fc** in the script with alikafka and click **OK**.

    ![pg_ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5105276061/p183450.png)


## Add permissions

To enable the MaxCompute sink connector to synchronize messages to a MaxCompute table, you must grant at least the following permissions to the RAM role.

|Object|Action|Description|
|------|------|-----------|
|Project|CreateInstance|The permissions to create instances in projects.|
|Table|Describe|The permissions to read the metadata of tables.|
|Table|Alter|The permissions to modify the metadata of tables and the permissions to create and delete partitions.|
|Table|Update|The permissions to overwrite data in tables and inset data to tables.|

For more information about the preceding permissions and how to grant these permissions, see [Authorize users](/intl.en-US/Management/Configure security features/Manage users and permissions/Authorize users.md).

To grant the required permissions to AliyunKafkaMaxComputeUser1, perform the following steps:

1.  Log on to the MaxCompute client.

2.  Run the following command to add the RAM user:

    ```
    add user `RAM$:$<accountid>:role/aliyunkafkamaxcomputeuser1`;
    ```

    **Note:** Replace <accountid\> with the UID of your Alibaba Cloud account.

3.  Grant the RAM user the minimum permissions that are required to access MaxComupte.

    1.  Run the following command to grant the RAM user the permissions on projects:

        ```
        grant CreateInstance on project connector_test to user `RAM$<accountid>:role/aliyunkafkamaxcomputeuser1`;
        ```

        **Note:** Replace <accountid\> with the UID of your Alibaba Cloud account.

    2.  Run the following command to grant the RAM user the permissions on tables:

        ```
        grant Describe, Alter, Update on table test_kafka to user `RAM$<accountid>:role/aliyunkafkamaxcomputeuser1`;
        ```

        **Note:** Replace <accountid\> with the UID of your Alibaba Cloud account.


## Create topics that are required by the MaxCompute sink connector.

In the Message Queue for Apache Kafka console, you can manually create five types of topics that are required by the MaxCompute sink connector.

1.  Log on to the [Message Queue for Apache KafkaConsole](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Topics**.

4.  In the **Topics**Page, select an instance and click **Create a topic**.

5.  In the **Create Topic** dialog box, configure the parameters for the topic and click **Create**.

    |Topic|Description|
    |-----|-----------|
    |Task site topic|The topic that is used to store consumer offsets.    -   Topic: the name of the topic. We recommend that you start the name with the string connect-offset.
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
    |Dead-letter queue topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, this topic and the abnormal data topic can be the same topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |
    |Abnormal data topic|The topic that is used to store the abnormal data of the sink connector. To save topic resources, this topic and the dead-letter queue topic can be the same topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |


## Create consumer groups that are required by the MaxCompute sink connector.

You can log on to the Message Queue for Apache Kafka console and manually create two consumer groups that are required by the MaxCompute sink connector.

1.  In the left-side navigation pane, click **Consumer Group management**.

2.  On the **Consumer Groups** page, select the instance and click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, configure the parameters and click **Create**.

    |Consumer Group|Description|
    |--------------|-----------|
    |Connector task consumer group|The name of the consumer group that is used by the data synchronization task. The name of this consumer group must be in the connect-task name format.|
    |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create and deploy a MaxCompute sink connector.

You can create and deploy a MaxCompute sink connector that is used to synchronize data from Message Queue for Apache Kafka to the MaxCompute sink connector.

1.  Log on to the [Message Queue for Apache KafkaConsole](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the **Connector**Page, select an instance and click **Create a connector.**.

4.  In the **Create Connector** pane, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list, select **MaxCompute** from the **Dump To** drop-down list, and then click **Next**.

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |Connector Name|The name of the connector. Naming conventions:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but must not start with a hyphen \(-\).
        -   Connector names must be unique for the same Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the format of connect-task name. If you have not created such a consumer group, the system automatically creates a consumer group for you.

|kafka-maxcompute-sink|
        |Task Type|The type of data synchronization task of the connector. In this example, the task synchronizes data from Message Queue for Apache Kafka to MaxCompute. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2ODPS|

    2.  In the **Configure Source Instance** step, enter a topic name in the **Data Source Topic** field, select a consumer offset from the **Consumer Offset** drop-down list, set the **Create Resource** parameter to **Automatically** or **Manually**, and then click **Next**. If you select Manually, enter a name for each manually created topic.

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |VPC ID|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to change the value.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|The ID of the vSwitch that is used in the data synchronization task. The vSwitch must be in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you have specified for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
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
        |Dead letter queue Topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, this topic and the Abnormal Data Topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-maxcompute-sink|
        |Abnormal Data Topic|The topic that is used to store the abnormal data of the sink connector. To save topic resources, this topic and the Dead letter queue Topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-maxcompute-sink|

    3.  In the **Configure Destination Instance** step, configure the parameters and click **Next**.

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |MaxCompute Endpoint|The endpoint of MaxCompute. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).         -   VPC endpoint: We recommend that you use the VPC endpoint because it has lower latency. The VPC endpoint can be used when the Message Queue for Apache Kafka instance and MaxCompute project are created in the same region.
        -   Public endpoint: We recommend that you do not use the public endpoint because it has higher latency. The public endpoint can be used when the Message Queue for Apache Kafka instance and the MaxCompute project are created in different regions. To use the public endpoint, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
        |MaxCompute Workspace|The name of the MaxCompute project to which you want to synchronize data.|connector\_test|
        |MaxCompute Table|The name of the MaxCompute table to which you want to synchronize data.|test\_kafka|
        |Region of MaxCompute Table|The region where the MaxCompute table is created.|China \(Hangzhou\)|
        |Alibaba Cloud Account|The UID of the Alibaba Cloud account that is used to access MaxCompute.|188\*\*\*|
        |RAM Role|The name of the RAM role assumed by Message Queue for Apache Kafka. For more information, see [Create a RAM role](#section_e02_70i_3jg).|AliyunKafkaMaxComputeUser1|
        |Mode|The mode in which messages are synchronized to the MaxCompute sink connector. The default value is DEFAULT. Valid values:        -   KEY: Only the keys of messages are retained and written into the Key column of the MaxCompute table.
        -   VALUE: Only the values of messages are retained and written into the Value column of the MaxCompute table.
        -   DEFAULT: Both keys and values of messages are retained and written into the Key and Value columns of the MaxCompute table respectively.

**Note:** In DEFAULT mode, the CSV format is not supported. You can select only the TEXT and BINARY formats.

|DEFAULT|
        |Format|The format in which messages are synchronized to the MaxCompute sink connector. The default value is TEXT. Valid values:        -   TEXT: strings.
        -   BINARY: byte arrays.
        -   CSV: strings separated with commas \(,\).

**Note:** If you select the CSV format, the DEFAULT mode is not supported. Only the KEY and VALUE modes are supported.

            -   KEY mode: Only the keys of messages are retained. Keys are separated with commas \(,\) and then written into the MaxCompute table in the order of indexes.
            -   VALUE mode: Only the values of messages are retained. Values are separated with commas \(,\) and then written into the MaxCompute table in the order of indexes.
|TEXT|
        |Partition|The granularity level at which partitions are created. The default value is HOUR. Valid values:        -   DAY: writes data into a new partition every day.
        -   HOUR: writes data into a new partition every hour.
        -   MINUTE: writes data into a new partition every minute.
|HOUR|
        |Time Zone|The time zone of the Message Queue for Apache Kafka producer client that sends messages to the source topic of the MaxCompute sink connector. The default value is GMT 08:00.|GMT 08:00|

    4.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

5.  In the **Create Connector** pane, click **Deploy**.


## Send test messages

After you deploy the MaxCompute sink connector, you can send messages to the source topic of Message Queue for Apache Kafka to test whether data can be synchronized to MaxCompute.

1.  On the **Connector** page, find the connector that you want to manage and click **Test** in the **Actions** column.

2.  On the **Topics** page, select the instance, find the **maxcompute-test-input** topic, and then choose ![point](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6199918061/p202912.png) \> **Send Message** in the **Actions** column.

3.  In the **Sends a message.**Dialog box, send a test message.

    1.  In the **Partitions**Text box, enter 0.

    2.  In the **Message Key**Text box, enter 1.

    3.  In the **Message Value**Text box, enter 1.

    4.  Click **Send**.


## View data in the MaxCompute table

After you send messages to the source topic of Message Queue for Apache Kafka, you can log on to the MaxCompute client and check whether the messages are received.

To view the test\_kafka table, perform the following steps:

1.  Log on to the MaxCompute client.

2.  Run the following command to view the partitions of the table:

    ```
    show partitions test_kafka;
    ```

    The following result is returned in this example:

    ```
    pt=11-17-2020 15
                                
    ```

3.  Run the following command to view the data stored in the partitions:

    ```
    select * from test_kafka where pt ="11-17-2020 14";
    ```

    The following result is returned in this example:

    ```
    +-------+------------+------------+-----+-------+----+ | topic | partition  | offset     | key | value | pt | +-------+------------+------------+-----+-------+----+ | maxcompute-test-input | 0          | 0          | 1   | 1     | 11-17-2020 14 | +-------+------------+------------+-----+-------+----+
    ```


