---
keyword: [kafka, connector, fc]
---

# Create a Function Compute sink connector

This topic describes how to create a Function Compute sink connector to export data from topics in your Message Queue for Apache Kafka instance to functions in Function Compute.

Before you create a Function Compute sink connector, ensure that you have completed the following operations:

1.  Enable the Connector feature for the Message Queue for Apache Kafka instance. For more information, see [Enable the Connector feature](/intl.en-US/User guide/Connectors/Enable the Connector feature.md).
2.  Create topics in the Message Queue for Apache Kafka instance. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    A topic named fc-test-input is used as an example.

3.  Create a function in Function Compute. For more information, see [Create a function in the Function Compute console]().

    An event function named hello\_world is used as an example. In this function, the service name is guide-hello\_world, and the runtime is Python. The following information shows the sample code of this function:

    ```
    # -*- coding: utf-8 -*-
    import logging
    
    # To enable the initializer feature
    # please implement the initializer function as below:
    # def initializer(context):
    #   logger = logging.getLogger()
    #   logger.info('initializing')
    
    def handler(event, context):
      logger = logging.getLogger()
      logger.info('hello world:' + bytes.decode(event))
      return 'hello world:' + bytes.decode(event)
    ```


## Procedure

To export data from topics in the Message Queue for Apache Kafka instance to a function in Function Compute by using a Function Compute sink connector, perform the following steps:

1.  Grant the Message Queue for Apache Kafka instance the permission to connect to Function Compute.
    1.  [Create a custom policy](#section_ein_kur_ed3)
    2.  [Create a RAM role](#section_onx_vzm_c6q)
    3.  [Add permissions](#section_ubx_dy2_thy)
2.  Optional. Create topics and consumer groups that the Function Compute sink connector requires.

    If you do not want to customize the names of the topics and consumer groups, skip this step and select Automatically in the next step.

    **Note:** Topics required by some Function Compute sink connectors must use a local storage engine. The Message Queue for Apache Kafka instance of major version 0.10.2 does not allow you to manually create topics by using a local storage engine. Instead, the topics must be automatically created.

    1.  [Optional. Create topics that a Function Compute sink connector requires](#section_0wn_cbs_hf5)
    2.  [Optional. Create consumer groups that a Function Compute sink connector requires](#section_fbf_mav_odr)
3.  Create and deploy the Function Compute sink connector.
    1.  [Create a Function Compute sink connector](#section_4dk_lib_xrh)
    2.  [Deploy the Function Compute sink connector](#section_h1f_aa2_ydg)
4.  Verify the result.
    1.  [Send messages](#section_rt2_26k_a0s)
    2.  [View function logs](#section_off_gyb_3lk)

## Create a custom policy

To create a custom policy for connecting to Function Compute, perform the following steps:

1.  Log on to the [Resource Access Management \(RAM\) console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

3.  On the **Policies** page, click **Create Policy**.

4.  On the **Create Custom Policy** page, create a custom policy.

    1.  In the **Policy Name** field, enter KafkaConnectorFcAccess.

    2.  Select **Script** for **Configuration Mode**.

    3.  In the **Policy Document** field, enter the custom policy script.

        The following information shows a sample custom policy script for accessing Function Compute:

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "fc:InvokeFunction",
                        "fc:GetFunction"
                    ],
                    "Resource": "*",
                    "Effect": "Allow"
                }
            ]
        }
        ```

    4.  Click **OK**.


## Create a RAM role

You cannot select Message Queue for Apache Kafka as the trusted service of a RAM role. Therefore, you must select any supported service as the trusted service when you create a RAM role. Then, you can manually modify the trust policy of the created RAM role.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, click **Create RAM Role**.

3.  In the **Create RAM Role** wizard, create a RAM role.

    1.  Select **Alibaba Cloud Service** for **Trusted entity type** and then click **Next**.

    2.  Select **Normal Service Role** for **Role Type**. In the **RAM Role Name** field, enter AliyunKafkaConnectorRole. Then, select **Function Compute** from the **Select Trusted Service** drop-down list and click **OK**.

4.  On the **RAM Roles** page, find and click **AliyunKafkaConnectorRole**.****

5.  On the **AliyunKafkaConnectorRole** page, click the **Trust Policy Management** tab, and then click **Edit Trust Policy**.

6.  In the **Edit Trust Policy** dialog box, replace **fc** in the script with alikafka and then click **OK**.

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6250549951/p128120.png)


## Add permissions

Grant the created RAM role the permission to access Function Compute.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **Add Permissions** in the **Actions** column.

3.  In the **Add Permissions** dialog box, add the **KafkaConnectorFcAccess** permission.

    1.  In the **Select Policy** section, click **Custom Policy**.

    2.  In the **Authorization Policy Name** column, find and click **KafkaConnectorFcAccess**.****

    3.  Click **OK**.

    4.  Click **Complete**.


## Optional. Create topics that a Function Compute sink connector requires

In the Message Queue for Apache Kafka console, you can manually create five topics for a Function Compute sink connector.

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


## Optional. Create consumer groups that a Function Compute sink connector requires

In the Message Queue for Apache Kafka console, you can manually create two consumer groups for a Function Compute sink connector.

1.  In the left-side navigation pane, click **Consumer Groups**.

2.  On the **Consumer Groups** page, click **Create Consumer Group**.

3.  In the **Create Consumer Group** dialog box, set attributes of the consumer group and then click **Create**.

    |Consumer Group|Description|
    |--------------|-----------|
    |Connector task consumer group|The consumer group that is used by data synchronization tasks. The name of this consumer group must be in the connect-task name format.|
    |Connector consumer group|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create a Function Compute sink connector

After you grant Message Queue for Apache Kafka the permission for accessing Function Compute, perform the following steps to create a Function Compute sink connector to synchronize data from Message Queue for Apache Kafka to Function Compute:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Connector**.

4.  On the **Connector** page, click **Create Connector**.

5.  On the **Create Connector** page, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list, select **Function Compute** from the **Dump To** drop-down list, and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Connector Name|The name of the connector. The value must comply with the following rules:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and dashes \(-\), but cannot start with a dash \(-\).
        -   The name must be unique within a Message Queue for Apache Kafka instance.
Data synchronization tasks of a connector must use a consumer group named connect-task name. If you have not manually created such a consumer group, the system automatically creates one.

|kafka-fc-sink|
        |Task Type|The type of the data synchronization task in the connector. In this example, the data synchronization task synchronizes data from Message Queue for Apache Kafka to Function Compute. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2FC|

    2.  In the **Configure Source Instance** step, enter a topic name in the **Data Source Topic** field, select a consumer offset from the **Consumer Offset** drop-down list, and then select **Automatically** or **Manually** for **Create Resource**. If you select Manually, you must enter the name of the manually created topic. Then, click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |VPC ID|The virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the VPC where the Message Queue for Apache Kafka instance resides. You do not need to set the parameter.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|The vSwitch where the data synchronization task runs. The vSwitch must be in the same VPC as the Message Queue for Apache Kafka instance. The default value is the vSwitch that you specified for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |Data Source Topic|The topic whose data needs to be synchronized.|fc-test-input|
        |Consumer Offset|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |Connector consumer group|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-fc-sink|
        |Task site Topic|The topic that is used to store consumer offsets.        -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. The value must be greater than 1.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-offset-kafka-fc-sink|
        |Task configuration Topic|The topic that is used to store task configurations.        -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. The value must be 1.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-config-kafka-fc-sink|
        |Task status Topic|The topic that is used to store task statuses.        -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value must be Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. The value must be compact.
|connect-status-kafka-fc-sink|
        |Dead letter queue Topic|The topic that is used to store abnormal data of the connector framework. To reduce topics, this topic can be reused as the abnormal data topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage.
|connect-error-kafka-fc-sink|
        |Abnormal Data Topic|The topic that is used to store abnormal data of the sink connector. To reduce topics, this topic can be reused as the dead-letter queue topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. The value can be Local Storage or Cloud Storage.
|connect-error-kafka-fc-sink|

    3.  In the **Configure Destination Instance** step, set attributes of the Function Compute service, set the Transmission Mode and Data Size parameters, and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Region|The region where Function Compute is located.|cn-hangzhou|
        |Service Endpoint|The endpoint of Function Compute. You can find the endpoint in the **Common Info** section on the **Overview** page in the Function Compute console.         -   Internal Endpoint: recommended for low latency. The internal endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in the same region.
        -   Public Endpoint: not recommended due to high latency. The public endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in different regions. To use the public endpoint, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
        |Alibaba Cloud Account|The ID of the Alibaba Cloud account that is used to log on to Function Compute. You can find the ID in the **Common Info** section on the **Overview** page in the Function Compute console.|188\*\*\*|
        |RAM Role|The name of the RAM role in Message Queue for Apache Kafka. For more information, see [Create a RAM role](#section_onx_vzm_c6q).|AliyunKafkaConnectorRole|
        |Service Name|The name of the Function Compute service.|guide-hello\_world|
        |Function Name|The name of the function in Function Compute.|hello\_world|
        |Service Version|The version of the Function Compute service.|LATEST|
        |Transmission Mode|The data transmission mode. Valid values:         -   Async: recommended.
        -   Sync: not recommended. In the synchronous transmission mode, if Function Compute takes a long time to process data, Message Queue for Apache Kafka will also take a long time to process data. If the processing time of the same batch of data exceeds 5 minutes, the Message Queue for Apache Kafka client will rebalance.
|Async|
        |Data Size|The number of messages that are sent at a time. Default value: 20. A synchronization task consolidates and transfers data based on this value and the limit for synchronous or asynchronous requests \(6 MB for synchronous requests and 128 KB for asynchronous requests\). If the size of a single data record exceeds the limit, the data will not be included in the request. This way, you can pull the Message Queue for Apache Kafka data by using an offset.|50|

6.  In the **Preview/Create** step, confirm the configurations of the connector and then click **Submit**.

    After you submit the configurations, refresh the **Connector** page. The connector that you created appears on the Connector page.


## Deploy the Function Compute sink connector

After you create a Function Compute sink connector, you must deploy it to synchronize data from Message Queue for Apache Kafka to Function Compute.

1.  On the **Connector** page, find the Function Compute sink connector that you created and then click **Deploy** in the **Actions** column.

    After you deploy the Function Compute sink connector, it enters the Running state.


## Send messages

After you deploy the Function Compute sink connector, you can send messages to topics in Message Queue for Apache Kafka to test whether the data can be synchronized to Function Compute.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, select an instance, find **fc-test-input**, and then click **Send Message** in the **Actions** column.

3.  In the **Send Message** dialog box, send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## View function logs

After you send a message to a topic in Message Queue for Apache Kafka, you can view the function log to check whether the message is received. For more information, see [Configure and view function logs]().

The log displays the sent test message.

![fc LOG](../images/p127831.png)

