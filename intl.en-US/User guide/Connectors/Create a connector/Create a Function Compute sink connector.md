---
keyword: [kafka, connector, fc]
---

# Create a Function Compute sink connector

This topic describes how to create a Function Compute sink connector to export data from topics in your Message Queue for Apache Kafka instance to functions in Function Compute.

Before you create a Function Compute sink connector, make sure that the following requirements are met:

1.  The connector feature for the Message Queue for Apache Kafka instance is enabled. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
2.  Data source topics for the Message Queue for Apache Kafka instance are created. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    A Topic named fc-test-input is used in this example.

3.  A function in Function Compute is created. For more information, see [Create a function in the Function Compute console]().

    **Note:** The created function must be an event function.

    An event function named hello\_world is used in this example. This is an event function under the guide-hello\_world service that runs in the Python runtime environment. The following information shows the sample code of this function:

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

To export data from a topic in the Message Queue for Apache Kafka instance to a function in Function Compute by using a Function Compute sink connector, perform the following steps:

1.  Allow Function Compute sink connector to access Function Compute across regions.

    **Note:** If you do not want Function Compute sink connector to access Function Compute across regions, you can skip this step.

    [Enable Internet access for Function Compute sink connector](#section_y3y_7cd_gpk)

2.  Allow Function Compute sink connector to access Function Compute across Alibaba Cloud accounts.

    **Note:** If you do not want Function Compute sink connector to access Function Compute across Alibaba Cloud accounts, you can skip this step.

    -   [Create a custom policy](#section_3wj_qkk_gwt)
    -   [Create a RAM role](#section_24p_yc7_s0d)
    -   [Add permissions](#section_co0_y32_ams)
3.  Create topics and consumer groups that are required by the Function Compute sink connector.

    **Note:**

    -   If you do not want to specify the names of the topics and consumer groups, you can skip this step.
    -   Some topics require a local storage engine. If the major version of your Message Queue for Apache Kafka instance is 0.10.2, you cannot manually create topics that use a local storage engine. In major version 0.10.2, these topics must be automatically created.
    1.  [Create topics that are required by the Function Compute sink connector.](#section_0wn_cbs_hf5)
    2.  [Create consumer groups](#section_fbf_mav_odr)
4.  [Create and deploy a Function Compute sink connector](#section_4dk_lib_xrh)
5.  Verify the result.
    1.  [Send test messages](#section_rt2_26k_a0s)
    2.  [View function logs](#section_off_gyb_3lk)

## Enable Internet access for Function Compute sink connector

If you want Function Compute sink connector to access other Alibaba Cloud services across regions, enable Internet access for Function Compute sink connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

## Create a custom policy

You can create a custom policy to access Function Compute by using the Alibaba Cloud account with which you want to use Function Compute. To create a custom policy, perform the following steps:

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

3.  On the **Policies** page, click **Create Policy**.

4.  On the **Create Custom Policy** page, create a custom policy.

    1.  In the **Policy Name** field, enter KafkaConnectorFcAccess.

    2.  Set **Configuration Mode** to **Script**.

    3.  In the **Policy Document** field, enter the custom policy script.

        The following code provides an example of the custom policy script that is used to access Function Compute:

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

You can create a RAM role by using the Alibaba Cloud account with which you want to use Function Compute. When you create the RAM role, select a supported service as the trusted service. You cannot select Message Queue for Apache Kafka as the trusted service of a RAM role. After you select a supported service, you can manually modify the trust policy of the created RAM role. To create a RAM role, perform the following steps:

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, click **Create RAM Role**.

3.  In the **Create RAM Role** pane, create a RAM role.

    1.  Set **Trusted entity type** to **Alibaba Cloud Service** and click **Next**.

    2.  Set **Role Type** to **Normal Service Role**. In the **RAM Role Name** field, enter AliyunKafkaConnectorRole. From the **Select Trusted Service** drop-down list, select **Function Compute**. Then, click **OK**.

4.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **AliyunKafkaConnectorRole**.

5.  On the **AliyunKafkaConnectorRole** page, click the **Trust Policy Management** tab, and then click **Edit Trust Policy**.

6.  In the **Edit Trust Policy** dialog box, replace **fc** with alikafka in the script and then click **OK**.

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6250549951/p128120.png)


## Add permissions

You can grant the created RAM role permissions to access Function Compute by using the Alibaba Cloud account with which you want to use Function Compute. To grant the created RAM role permissions, perform the following steps:

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **Add Permissions** in the **Actions** column.

3.  In the **Add Permissions** dialog box, add the **KafkaConnectorFcAccess** permission.

    1.  In the **Select Policy** section, click **Custom Policy**.

    2.  In the **Authorization Policy Name** column, find **KafkaConnectorFcAccess** and click **KafkaConnectorFcAccess**.

    3.  Click **OK**.

    4.  Click **Complete**.


## Create topics that are required by the Function Compute sink connector.

In the Message Queue for Apache Kafka console, you can manually create five types of topics that are required by the Function Compute sink connector.

1.  Log on to the [Message Queue for Apache KafkaConsole](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Topics**.

4.  In the **Topics**Page, select an instance and click **Create a topic**.

5.  In the **Create Topic** dialog box, set the parameters for the topic and click **Create**.

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
    |Task status topic|The topic that is used to store task status.    -   Topic: the name of the topic. We recommend that you start the name with connect-status.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact. |
    |Dead-letter queue topic|The topic that is used to store abnormal data of the connector framework. To save topic resources, this topic and the abnormal data topic can be the same topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |
    |Abnormal data topic|The topic that is used to store abnormal data of the sink connector. To save topic resources, this topic and the dead-letter queue topic can be the same topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |


## Create consumer groups

In the Message Queue for Apache Kafka console, you can manually create the two consumer groups that are required by the Function Compute sink connector.

1.  In the left-side navigation pane, click **Consumer Group management**.

2.  In the **Consumer Group management**Page, select an instance and click **Create consumer groups**.

3.  In the **Create Consumer Group** dialog box, configure the parameters and click **Create**.

    |Consumer Group|Description|
    |--------------|-----------|
    |Connector task consumer group|The name of the consumer group that is used by the data synchronization task. The name of this consumer group must be in the connect-task name format.|
    |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create and deploy a Function Compute sink connector

You can create and deploy a Function Compute sink connector that synchronizes data from Message Queue for Apache Kafka to Function Compute.

1.  Log on to the [Message Queue for Apache KafkaConsole](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Connector**.

4.  In the **Connector**Page, select an instance and click **Create a connector.**.

5.  In the **Create Connector** pane, perform the following steps:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list, select **Function Compute** from the **Dump To** drop-down list, and then click **Next**.

        **Note:** Message Queue for Apache Kafka automatically selects **Authorize to create service linked roles**.

        -   If no service linked role is created, Message Queue for Apache Kafka automatically creates a service linked role to use Function Compute sink connector.
        -   If a service-linked role is created, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service linked roles](/intl.en-US/Access control/Service linked roles.md).

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |Connector Name|The name of the connector. The name must meet the following requirements:        -   The name of the connector must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but must not start with a hyphen \(-\).
        -   The names of the connectors must be unique for the same Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the format of connect-task name. If this consumer group is not manually created, the system automatically creates a consumer group.

|kafka-fc-sink|
        |Task Type|The type of data synchronization task of the connector. In this example, the data synchronization task synchronizes data from Message Queue for Apache Kafka to Function Compute. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2FC|

    2.  In the **Configure Source Instance** step, enter a topic name in the **Data Source Topic** field, select a consumer offset from the **Consumer Offset** drop-down list, set **Create Resource** to **Automatically** or **Manually**, and then click **Next**. If you select Manually, enter the name of the topic that you manually created.

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |VPC ID|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to change the value.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|The ID of the vSwitch based on which the data synchronization task runs. The vSwitch must be in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you specify for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |Data Source Topic|The topic from which data is to be synchronized.|fc-test-input|
        |Consumer Offset|The offset from which consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |Consumer Thread Concurrency|The consumer thread concurrency of the data source topic. Default value: 3. Valid values:        -   3
        -   6
        -   9
        -   12
|3|
        |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-fc-sink|
        |Task site Topic|The topic that is used to store consumer offsets.        -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. The value must be greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-offset-kafka-fc-sink|
        |Task configuration Topic|The topic that is used to store task configurations.        -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the value to 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-config-kafka-fc-sink|
        |Task status Topic|The topic that is used to store task status.        -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-status-kafka-fc-sink|
        |Dead-letter queue Topic|The topic that is used to store abnormal data of the connector framework. To save topic resources, this topic and the abnormal data topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-fc-sink|
        |Abnormal Data Topic|The topic that is used to store abnormal data of the sink connector. To save topic resources, this topic and the dead-letter queue topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-fc-sink|

    3.  In the **Configure Destination Instance** step, configure the parameters for the Function Compute service, configure Transmission Mode and Data Size parameters, and then click **Next**.

        |Parameter|Description|Example value|
        |---------|-----------|-------------|
        |Cross-account/Cross-region|Specifies whether the Function Compute sink connector synchronizes data to Function Compute across Alibaba Cloud accounts or regions. The default value is **No**. Valid values:        -   No: The Function Compute sink connector synchronizes data to Function Compute by using the same Alibaba Cloud account and in the same region.
        -   Yes: The Function Compute sink connector synchronizes data to Function Compute in the same region by using different accounts, across regions by using the same account, or across regions by using different accounts.
|No|
        |Region|The region where Function Compute is located. The region where the Function Compute sink connector is deployed is selected by default. To allow cross-region synchronization, enable Internet access for the Connector and select a region. For more information, see [Enable Internet access for Function Compute sink connector](#section_y3y_7cd_gpk).**Note:** You must select a value for Region when Cross-account/Cross-region is set to **Yes**.

|cn-hangzhou|
        |Service Endpoint|The endpoint that is used to access the Function Compute service. In the [Function Compute](https://fc.console.aliyun.com/fc/overview/cn-hangzhou) console, you can view the endpoint of Function Compute in the **Common Info** section on the **Overview** page.         -   Internal Endpoint: The internal endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in the same region. We recommend that you use the internal endpoint because it has lower latency.
        -   Public Endpoint: The public endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute instance are in different regions. We recommend that you do not use the public endpoint because it has high latency. To use the Public Endpoint value, enable Internet access for the connector. For more information, see [Enable Internet access for Function Compute sink connector](#section_y3y_7cd_gpk).
**Note:** You must specify a value for Endpoint when Cross-account/Cross-region is set to **Yes**.

|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
        |Alibaba Cloud Account|The ID of the Alibaba Cloud account that is used to log on to Function Compute. In the Function Compute console, you can view the Alibaba Cloud account in the **Common Info** section on the **Overview** page.**Note:** You must specify a value for Alibaba Cloud Account when Cross-account/Cross-region is set to **Yes**.

|188\*\*\*|
        |RAM Role|The name of the RAM role that Message Queue for Apache Kafka is authorized to assume to access Function Compute.        -   If you do not need to use a different Alibaba Cloud account, you must create a RAM role and grant the RAM role the permissions. Then, enter the name of the RAM role. For more information, see [Create a custom policy](#section_3wj_qkk_gwt), [Create a RAM role](#section_24p_yc7_s0d), and [Add permissions](#section_co0_y32_ams).
        -   If you want to use a different Alibaba Cloud account, you must create a RAM role under the account with which you want to use Function Compute. Then, grant the RAM role permissions and enter the name of the RAM role. For more information, see [Create a custom policy](#section_3wj_qkk_gwt), [Create a RAM role](#section_24p_yc7_s0d), and [Add permissions](#section_co0_y32_ams).
**Note:** You must specify a value for RAM Role when Cross-account/Cross-region is set to **Yes**.

|AliyunKafkaConnectorRole|
        |Service Name|The name of the Function Compute service that contains the function that you created..|guide-hello\_world|
        |Function Name|The name of the function in Function Compute.|hello\_world|
        |Service Version or Alias|The version or alias of the service in Function Compute.|LATEST|
        |Transmission Mode|The message delivery mode. Valid values:         -   Async: recommended.
        -   Sync: not recommended. In synchronous transmission mode, if Function Compute takes a long time to process data, Message Queue for Apache Kafka also take a long time to process data. If it takes longer than five minutes to process multiple messages in a batch, the Message Queue for Apache Kafka client rebalances the traffic.
|Async|
        |Data Size|The size of the data to be transmitted. Default value: 20. The connector aggregates multiple messages into batches based on this value. A batch cannot exceed 6 MB in synchronous mode and 128 KB in asynchronous mode. Assume that the batch size is 20 and the sending mode is asynchronous. The first 17 messages have a total size of 127 KB, but the 18th message is 200 KB. In this case, the connector sends the first 17 messages as a single batch, and then separately sends the 18th message.**Note:** If you set key to null when you send a message, the request does not contain the key. If the value is set to null, the request does not contain a value.

        -   If the messages in a batch do not exceed the size limit, the request contains the content of the messages. The following code provides a sample request:

            ```
[
    {
        "key":"this is the message's key2",
        "offset":8,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325438,
        "topic":"Test",
        "value":"this is the message's value2",
        "valueSize":28
    },
    {
        "key":"this is the message's key9",
        "offset":9,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325440,
        "topic":"Test",
        "value":"this is the message's value9",
        "valueSize":28
    },
    {
        "key":"this is the message's key12",
        "offset":10,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325442,
        "topic":"Test",
        "value":"this is the message's value12",
        "valueSize":29
    },
    {
        "key":"this is the message's key38",
        "offset":11,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325464,
        "topic":"Test",
        "value":"this is the message's value38",
        "valueSize":29
    }
]
            ```

        -   If a single message exceeds the size limit, the request does not contain the content of the message. The following code provides a sample request:

            ```
[
    {
        "key":"123",
        "offset":4,
        "overflowFlag":true,
        "partition":0,
        "timestamp":1603779578478,
        "topic":"Test",
        "value":"1",
        "valueSize":272687
    }
]
            ```

**Note:** To obtain the content of the message, pull the message by using the offset of the message.

|50|
        |Retries|The number of retries after a message failed to be sent. Default value: 2. Valid values: 1 to 3. Partial message delivery failures do not support retries. The following list describes the types of error codes that support retries:        -   4XX: does not support a retry except for 429.
        -   5XX: supports a retry.
**Note:**

        -   For more information about error codes, see [Error codes]().
        -   The connector calls the InvokeFunction operation to send messages to Function Compute. For more information about InvokeFunction, see [List of operations by function]().
|2|
        |Failure Handling Policy|The solution to message delivery failures. Default value: log. Valid values:        -   log: You remain subscribed to the partition in which the error occurs and the error is printed in the log. After an error occurs, you can view the error in the Connector log and find a solution based on the error code. This allows you to troubleshoot the error.

**Note:**

            -   For more information about how to view Connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to search for solutions based on error codes, see[Error codes]().
        -   fail: You are no longer subscribed to the partition in which the error occurs and the error is printed in the log. After an error occurs, you can view the error in the Connector log and find a solution based on the error code. This allows you to troubleshoot the error.

**Note:**

            -   For more information about how to view Connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to search for solutions based on error codes, see [Error codes]().
            -   To resume subscription to the partition in which an error occurs,[submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to request technical support of Message Queue for Apache Kafka.
|log|

    4.  In the **Preview/Submit** page, confirm the configurations of the connector and click **Submit**.

6.  In the **Create Connector** pane, click **Deploy**.


## Send test messages

After you deploy the Function Compute sink connector, you can send messages to topics in Message Queue for Apache Kafka to test whether the data can be synchronized to Function Compute.

1.  On the **Connector** page, find the connector you want to manage and click **Test** in the **Actions** column.

2.  On the **Topics** page, click the instance that contains the fc-test-input topic, find **fc-test-input**, and then choose ![point](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6199918061/p202912.png) \> **Send Message** in the **Actions** column.

3.  In the **Sends a message.**Dialog box, send a test message.

    1.  In the **Partitions**Text box, enter 0.

    2.  In the **Message Key**Text box, enter 1.

    3.  In the **Message Value**Text box, enter 1.

    4.  Click **Send**.


## View function logs

After you send a message to the data source topic in Message Queue for Apache Kafka, you can view the function logs to check whether the message is received. For more information, see [Configure and view function logs]().

The test message that is sent appears in the logs.

![fc LOG](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2228646061/p127831.png)

