---
keyword: [kafka, connector, fc]
---

# Create a Function Compute sink connector

This topic explains how to create a Function Compute sink connector to export data from Message Queue for Apache Kafka to Function Compute.

Before you create a Function Compute sink connector, ensure that you have completed the following operations:

1.  Purchase a Message Queue for Apache Kafka instance and deploy. For more information, see [Connect Message Queue for Apache Kafka to a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).
2.  Enable the connector feature for the Message Queue for Apache Kafka instance. For more information, see [Enable the Connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
3.  Create an event function in Function Compute. Set the service name to guide-hello\_world, the function name to hello\_world, and the runtime environment to Python. After you create the function, modify the following function code:

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

    For more information, see [Create a function in the Function Compute console]().


## Procedure

Complete the following steps to export data from a topic in Message Queue for Apache Kafka to Function Compute by using a Function Compute sink connector:

1.  Grant Message Queue for Apache Kafka the permission to access Function Compute.
    1.  [Create a custom policy](#section_ein_kur_ed3)
    2.  [Create a RAM role](#section_onx_vzm_c6q)
    3.  [Add permissions](#section_ubx_dy2_thy)
2.  Create the Message Queue for Apache Kafka resources that a Function Compute sink connector requires.
    1.  [Create consumer groups](#section_876_je5_j2f)
    2.  [Create topics](#section_17l_v1q_zyx)
3.  Create a Function Compute sink connector.

    [Create a Function Compute sink connector](#section_4dk_lib_xrh)

4.  Verify the result.
    1.  [Send messages](#section_rt2_26k_a0s)
    2.  [View function logs](#section_off_gyb_3lk)

## Create a custom policy

Create a custom policy for accessing Function Compute.

1.  Log on to the [Resource Access Management \(RAM\) console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

3.  On the **Policies** page, click **Create Policy**.

4.  On the **Create Custom Policy** page, create a custom policy.

    1.  In the **Policy Name** field, enter KafkaConnectorFcAccess.

    2.  Select **Script** for **Configuration Mode**.

    3.  In the **Policy Document** field, enter the custom policy script.

        The following code provides an example of a custom policy script for Function Compute access:

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

You cannot directly select Message Queue for Apache Kafka as the trusted service of a RAM role. Therefore, select any support services as the trusted service when you create a RAM role. Then, manually modify the trust policy of the created RAM role.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, click **Create RAM Role**.

3.  In the **Create RAM Role** wizard, create a RAM role.

    1.  Select **Alibaba Cloud Service** for **Trusted Entity Type** and click **Next**.

    2.  Select **Normal Service Role** for **Role Type**. In the **RAM Role Name** field, enter AliyunKafkaConnectorRole. Then, choose **Function Compute** from the **Select Trusted Service** drop-down list, and click **OK**.

4.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **AliyunKafkaConnectorRole**.

5.  On the **AliyunKafkaConnectorRole** page, click the **Trust Policy Management** tab, and then click **Edit Trust Policy**.

6.  In the **Edit Trust Policy** pane, replace **fc** in the script with alikafka and click **OK**.

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6250549951/p128120.png)


## Add permissions

Grant the created RAM role the permission to access Function Compute.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **Add Permissions** in the **Actions** column.

3.  In the **Add Permissions** pane, add the **KafkaConnectorFcAccess** permission.

    1.  In the **Select Policy** section, click **Custom Policy**.

    2.  In the **Authorization Policy Name** column, find **KafkaConnectorFcAccess** and click **KafkaConnectorFcAccess**.

    3.  Click **OK**.

    4.  Click **Complete**.


## Create consumer groups

After you have granted the RAM role in Message Queue for Apache Kafka the permission to access Function Compute, create consumer groups for performing data synchronization tasks by using a Function Compute sink connector.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Consumer Groups**.

4.  On the **Consumer Groups** page, click **Create Consumer Group**.

5.  In the **Create Consumer Group** dialog box, create a consumer group.

    1.  In the **Consumer Group** field, enter connect-kafka-fc-sink.

    2.  In the **Description** field, enter connect-kafka-fc-sink.

    3.  Click **Create**.

6.  Create a connector consumer group by repeating Steps 4 and 5.

    |Consumer group type|Parameter|Description|Example|
    |-------------------|---------|-----------|-------|
    |Connector consumer group|Consumer Group|The name of the consumer group. We recommend that you start the name with connect-cluster.|connect-cluster-kafka-fc-sink|

    The created consumer group appears on the **Consumer Groups** page.


## Create topics

Create topics for performing data synchronization tasks by using a Function Compute sink connector.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, click **Create Topic**.

3.  In the **Create Topic** dialog box, create a data source topic.

    1.  In the **Topic** field, enter fc-test-input.

    2.  In the **Description** field, enter fc-test-input.

    3.  Click **Create**.

4.  Create the following topic by repeating Steps 2 and 3.

    |Topic type|Parameter|Description|Example|
    |----------|---------|-----------|-------|
    |Task site topic|Topic|The name of the topic. We recommend that you start the name with connect-offset.|connect-offset-fc-sink|
    |Partitions|The number of partitions in the topic. The value must be greater than 1.|12|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Task configuration topic|Topic|The name of the topic. We recommend that you start the name with connect-config.|connect-config-fc-sink|
    |Partitions|The number of partitions in the topic. The value must be 1.|1|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Task status topic|Topic|The name of the topic. We recommend that you start the name with connect-status.|connect-status-fc-sink|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value must be Local Storage.|Local Storage|
    |cleanup.policy|The log cleanup policy for the topic. The value must be compact.|compact|
    |Dead-letter queue topic|Topic|The name of the topic.|fc\_dead\_letter\_error|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value can be Local Storage or Cloud Storage.|Cloud Storage|
    |Abnormal data topic|Topic|The name of the topic.|fc\_runtime\_error|
    |Partitions|The number of partitions in the topic. We recommend that you set the value to 6.|6|
    |Storage Engine|The storage engine of the topic. The value can be Local Storage or Cloud Storage.|Cloud Storage|

    **Note:** You can combine the dead-letter queue topic and the abnormal data topic into one to store abnormal data.

    The created topics appear on the **Topics** page.


## Create a Function Compute sink connector

After you create the consumer groups and topics for performing data synchronization tasks by using a Function Compute sink connector, create a Function Compute sink connector to export data from the topics in Message Queue for Apache Kafka to Function Compute.

1.  In the left-side navigation pane, click **Connector**.

2.  On the **Connector** page, click **Create Connector**.

3.  On the **Create Connector** page, enter the connector information and click **Pre-Check and Create**.

    |Section|Parameter|Description|Example|
    |-------|---------|-----------|-------|
    |Task Information|Task Name|The name of the data synchronization task. The name must be unique within the instance. The synchronization tasks that are performed by using a connector use the consumer groups named in the connect-task name format. Therefore, before you create a connector, create consumer groups that are named in the connect-task name format.|kafka-fc-sink|
    |General Information|Task Type|The type of the data synchronization task. For more information about the task types that Message Queue for Apache Kafka supports, see [Types of connectors](/intl.en-US/User guide/Connectors/Connectors overview.md).|KAFKA2FC|
    |User VPC|The virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the VPC where the Message Queue for Apache Kafka instance is deployed, and you do not need to enter the value.|vpc-bp1xpdnd3l\*\*\*|
    |VSwitch|The VSwitch where the data synchronization task runs. The VSwitch must be on the same VPC as the Message Queue for Apache Kafka instance. The default value is the VSwitch that you specified for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
    |Source Instance Information|Data Source Topic|The topic whose data needs to be synchronized.|fc-test-input|
    |Consumer Offset|The offset where consumption starts. Valid values:     -   latest: Consumption starts from the latest offset.
    -   earliest: Consumption starts from the initial offset.
|latest|
    |Connector Consumer Group|The consumer group that is used for data synchronization.|connect-cluster-kafka-fc-sink|
    |Task Site Topic|The topic that is used to store consumer offsets.|connect-offset-fc-sink|
    |Task Configuration Topic|The topic that is used to store task configurations.|connect-config-fc-sink|
    |Task Status Topic|The topic that is used to store task statuses.|connect-status-fc-sink|
    |Dead-letter Queue Topic|The topic that is used to store the abnormal data of the connector framework.|fc\_dead\_letter\_error|
    |Abnormal Data Topic|The topic that is used to store the abnormal data of the sink connector.|fc\_runtime\_error|
    |Destination Instance Information|Region|The region where Function Compute is located.|cn-hangzhou|
    |Service Endpoint|The endpoint of Function Compute. You can find the endpoint in the **Common Info** section on the **Overview** page in the Function Compute console.     -   Internal Endpoint: recommended for low latency. The internal endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in the same region.
    -   Public Endpoint: not recommended due to high latency. The Public Endpoint value can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in different regions. To use the Public Endpoint value, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).
|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
    |Alibaba Cloud Account|The ID of the Alibaba Cloud account that is used to log on to Function Compute. You can find the ID in the **Common Info** section on the **Overview** page in the Function Compute console.|188\*\*\*|
    |RAM Role|The name of the RAM role in Message Queue for Apache Kafka. For more information, see [Create a RAM role](#section_onx_vzm_c6q).|AliyunKafkaConnectorRole|
    |Service Name|The name of the Function Compute service.|guide-hello\_world|
    |Function Name|The function name of the Function Compute service.|hello\_world|
    |Service Version|The version of the Function Compute service.|LATEST|
    |Transmission Mode|The data transmission mode. Valid values:     -   Asynchronous: recommended.
    -   Synchronous: not recommended. In the synchronous transmission mode, if Function Compute takes a long time to process data, Message Queue for Apache Kafka will also take a long time to process data. If the processing time of the same batch of data exceeds 5 minutes, the Message Queue for Apache Kafka client will rebalance.
|Asynchronous|
    |Data Size|The number of messages that are sent in a batch. Default value: 20. A synchronization task consolidates and transfers data based on the combination of this value and the maximum size limits for synchronous or asynchronous requests \(6 MB for synchronous requests and 128 KB for asynchronous requests\). If the size of a single data record exceeds the limit, the data will not be included in the request. In this case, you can pull the Message Queue for Apache Kafka data by using an offset.|50|

    The created Function Compute sink connector appears on the **Connector** page.


## Send messages

After you create a Function Compute sink connector to connect Message Queue for Apache Kafka and Function Compute, send a test message to the data source topic in Message Queue for Apache Kafka.

1.  In the left-side navigation pane, click **Topics**.

2.  On the **Topics** page, find **fc-test-input** and click **Send Message** in the **Actions** column.

3.  In the **Send Message** dialog box, send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## View function logs

After you send the message to the data source topic in Message Queue for Apache Kafka, view the function log to check whether the message is received. For more information, see [Configure and view function logs]().

The sent test message appears in the log.

-   [View the task configurations of a connector](/intl.en-US/User guide/Connectors/View the task configurations of a connector.md)
-   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md)
-   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md)
-   [Modify the connector description](/intl.en-US/User guide/Connectors/Modify connector descriptions.md)

