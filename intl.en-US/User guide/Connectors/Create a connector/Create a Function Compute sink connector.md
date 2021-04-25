---
keyword: [kafka, connector, fc]
---

# Create a Function Compute sink connector

This topic describes how to create a Function Compute sink connector to synchronize data from a topic in your Message Queue for Apache Kafka instance to a function in Function Compute.

Before you create a Function Compute sink connector, make sure that the following operations are completed:

-   The connector feature is enabled for the Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
-   A data source topic is created in the Message Queue for Apache Kafka instance . For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    A topic named fc-test-input is used in this example.

-   A function is created in Function Compute. For more information, see [Create a function in the Function Compute console]().

    **Note:** The function you create must be an event function.

    An event function named hello\_world is used in this example. This is an event function under the guide-hello\_world service that runs in the Python runtime environment. The following sample code provides an example of the function:

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

To synchronize data from a topic in your Message Queue for Apache Kafka instance to a function in Function Compute by using a Function Compute sink connector, perform the following steps:

1.  Enable Function Compute sink connectors to access Function Compute across regions.

    **Note:** If you do not want Function Compute sink connectors to access Function Compute across regions, skip this step.

    [Enable Internet access for Function Compute sink connectors](#section_y3y_7cd_gpk)

2.  Enable Function Compute sink connectors to access Function Compute across Alibaba Cloud accounts.

    **Note:** If you do not want Function Compute sink connectors to access Function Compute across Alibaba Cloud accounts, skip this step.

    -   [Create a custom policy](#section_3wj_qkk_gwt)
    -   [Create a RAM role](#section_24p_yc7_s0d)
    -   [Grant permissions](#section_co0_y32_ams)
3.  Create the topics and consumer groups that are required by a Function Compute sink connector.

    **Note:**

    -   If you do not want to customize the names of the topics and consumer groups, skip this step.
    -   Some topics that are required by a Function Compute sink connector must use a local storage engine. If the major version of your Message Queue for Apache Kafka instance is 0.10.2, you cannot manually create topics that use a local storage engine. In major version 0.10.2, these topics must be automatically created.
    1.  [Create topics for a Function Compute sink connector](#section_0wn_cbs_hf5)
    2.  [Create consumer groups for a Function Compute sink connector](#section_fbf_mav_odr)
4.  [Create and deploy a Function Compute sink connector](#section_4dk_lib_xrh)
5.  Verify the results.
    1.  [Send a test message](#section_rt2_26k_a0s)
    2.  [View function logs](#section_off_gyb_3lk)

## Enable Internet access for Function Compute sink connectors

If you want Function Compute sink connectors to access other Alibaba Cloud services across regions, enable Internet access for Function Compute sink connectors. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

## Create a custom policy

You can create a custom policy to access Function Compute by using the Alibaba Cloud account within which you want to use Function Compute.

1.  Log on to the [Resource Access Management \(RAM\) console](https://ram.console.aliyun.com/).

2.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

3.  On the **Policies** page, click **Create Policy**.

4.  On the **Create Custom Policy** page, create a custom policy.

    1.  In the **Policy Name** field, enter KafkaConnectorFcAccess.

    2.  Set **Configuration Mode** to **Script**.

    3.  In the **Policy Document** field, enter the custom policy script.

        The following sample code provides an example of the custom policy script for access to Function Compute:

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

You can create a RAM role by using the Alibaba Cloud account within which you want to use Function Compute. When you create the RAM role, select a supported Alibaba Cloud service as the trusted service. You cannot select Message Queue for Apache Kafka as the trusted service of a RAM role. After you create the RAM role, you can modify the trust policy of the created RAM role.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, click **Create RAM Role**.

3.  In the **Create RAM Role** panel, create a RAM role.

    1.  Set **Trusted entity type** to **Alibaba Cloud Service** and click **Next**.

    2.  Set **Role Type** to **Normal Service Role**. In the **RAM Role Name** field, enter AliyunKafkaConnectorRole. From the **Select Trusted Service** drop-down list, select **Function Compute**. Then, click **OK**.

4.  On the **RAM Roles** page, find and click **AliyunKafkaConnectorRole**.

5.  On the **AliyunKafkaConnectorRole** page, click the **Trust Policy Management** tab, and then click **Edit Trust Policy**.

6.  In the **Edit Trust Policy** panel, replace **fc** in the script with alikafka and click **OK**.

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6250549951/p128120.png)


## Grant permissions

You can grant the created RAM role the permissions to access Function Compute by using the Alibaba Cloud account within which you want to use Function Compute.

1.  In the left-side navigation pane, click **RAM Roles**.

2.  On the **RAM Roles** page, find **AliyunKafkaConnectorRole** and click **Add Permissions** in the **Actions** column.

3.  In the **Add Permissions** panel, attach the **KafkaConnectorFcAccess** policy.

    1.  In the **Select Policy** section, click **Custom Policy**.

    2.  In the **Authorization Policy Name** column, find and click **KafkaConnectorFcAccess**.****

    3.  Click **OK**.

    4.  Click **Complete**.


## Create topics for a Function Compute sink connector

In the Message Queue for Apache Kafka console, you can manually create the five topics that are required by a Function Compute sink connector.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Topics**.

6.  On the **Topics** page, click **Create Topic**.

7.  In the **Create Topic** panel, set the properties of a topic and click **Create**.

    |Topic|Description|
    |-----|-----------|
    |Offset storage topic|The topic that is used to store consumer offsets.     -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
    -   Partitions: the number of partitions in the topic. Set it to a value greater than 1.
    -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact. |
    |Task configuration topic|The topic that is used to store task configurations.     -   Topic: the name of the topic. We recommend that you start the name with connect-config.
    -   Partitions: the number of partitions in the topic. Set the parameter to 1.
    -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact. |
    |Task status topic|The topic that is used to store task status.     -   Topic: the name of the topic. We recommend that you start the name with connect-status.
    -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
    -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
    -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact. |
    |Dead-letter queue topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, you can create a topic as both the dead-letter queue topic and the abnormal data topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |
    |Abnormal data topic|The topic that is used to store the abnormal data of the connector. To save topic resources, you can create a topic as both the dead-letter queue topic and the abnormal data topic.    -   Topic: the name of the topic. We recommend that you start the name with connect-error.
    -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
    -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage. |


## Create consumer groups for a Function Compute sink connector

In the Message Queue for Apache Kafka console, you can create the two consumer groups that are required by a Function Compute sink connector.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Consumer Groups**.

6.  On the **Consumer Groups** page, click **Create Consumer Group**.

7.  In the **Create Consumer Group** panel, set the properties of a consumer group and click **Create**.

    |Consumer Group|Description|
    |--------------|-----------|
    |Connector task consumer group|The consumer group that is used by the data synchronization task of the connector. The name of this consumer group must be in the connect-Task name format.|
    |Connector consumer group|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|


## Create and deploy a Function Compute sink connector

You can create and deploy a Function Compute sink connector that synchronizes data from Message Queue for Apache Kafka to Function Compute.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, click **Create Connector**.

7.  In the **Create Connector** panel, perform the following steps:

    1.  In the **Enter Basic Information** step, set the parameters that are described in the following table, and click **Next**.

        **Note:** By default, the **Authorize to Create Service Linked Role** check box is selected. This means that Message Queue for Apache Kafka will create a service-lined role based on the following rules:

        -   If no service-linked role is created, Message Queue for Apache Kafka automatically creates a service-linked role for you to use the Function Compute sink connector.
        -   If you have created a service-linked role, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md).

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Connector Name**|The name of the connector. Take note of the following rules when you specify a connector name:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The connector name must be unique within a Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the connect-Task name format. If you have not manually created such a consumer group, the system automatically creates a consumer group for you.

|kafka-fc-sink|
        |**Dump Path**|The source and destination of data transfer. Select a data source from the first drop-down list and a destination from the second drop-down list.|**Message Queue for Apache Kafka** Dump To **Function Compute**|

    2.  In the **Configure Source Instance** step, set the parameters that are described in the following table, and click **Next**.

        **Note:** If you have created the required topics and consumer groups, set Create Resource to Manually and enter the information about the created resources. Otherwise, set Create Resource to Automatically.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**VPC ID**|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to enter a value.|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch**|The ID of the vSwitch based on which the data synchronization task runs. The vSwitch must be deployed in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you specify for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |**Data Source Topic**|The name of the topic from which data is to be synchronized.|fc-test-input|
        |**Consumer Offset**|The offset where consumption starts. Valid values:         -   **latest**: Consumption starts from the latest offset.
        -   **earliest**: Consumption starts from the initial offset.
|latest|
        |**Consumer Thread Concurrency**|The number of concurrent consumer threads to synchronize data from the data source topic. Default value: 3. Valid values:        -   **3**
        -   **6**
        -   **9**
        -   **12**
|3|
        |**Connector consumer group**|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-fc-sink|
        |**Task site Topic**|The topic that is used to store consumer offsets.         -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. Set it to a value greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-offset-kafka-fc-sink|
        |**Task configuration Topic**|The topic that is used to store task configurations.         -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the parameter to 1.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-config-kafka-fc-sink|
        |**Task status Topic**|The topic that is used to store task status.         -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-status-kafka-fc-sink|
        |**Dead letter queue Topic**|The topic that is used to store the abnormal data of the connector framework. To save topic resources, you can create a topic as both the dead-letter queue topic and the abnormal data topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-fc-sink|
        |**Abnormal Data Topic**|The topic that is used to store the abnormal data of the connector. To save topic resources, you can create a topic as both the dead-letter queue topic and the abnormal data topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-fc-sink|

    3.  In the **Configure Destination Instance** step, set the parameters that are described in the following table, and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Cross-account/Cross-region**|Specifies whether the Function Compute sink connector synchronizes data to Function Compute across Alibaba Cloud accounts or regions. By default, this parameter is set to **No**. Valid values:        -   **No**: The Function Compute sink connector synchronizes data to Function Compute by using the same Alibaba Cloud account and in the same region.
        -   **Yes**: The Function Compute sink connector synchronizes data to Function Compute across regions by using the same Alibaba Cloud account, in the same region by using different Alibaba Cloud accounts, or across regions by using different Alibaba Cloud accounts.
|No|
        |**Region**|The region where Function Compute is activated. By default, the region where the Function Compute sink connector resides is selected. To synchronize data across regions, enable Internet access for the connector and select the destination region. For more information, see [Enable Internet access for Function Compute sink connectors](#section_y3y_7cd_gpk). **Note:** When **Cross-account/Cross-region** is set to **Yes**, **Region** is displayed.

|cn-hangzhou|
        |**Service Endpoint**|The endpoint of Function Compute. In the [Function Compute](https://fc.console.aliyun.com/fc/overview/cn-hangzhou) console, you can view the endpoint of Function Compute in the **Common Info** section on the **Overview** page.         -   Internal endpoint: We recommend that you use the internal endpoint because it has lower latency. The internal endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute are in the same region.
        -   Public endpoint: We recommend that you do not use the public endpoint because it has high latency. The public endpoint can be used when the Message Queue for Apache Kafka instance and Function Compute are in different regions. To use the public endpoint, you must enable Internet access for the connector. For more information, see [Enable Internet access for Function Compute sink connectors](#section_y3y_7cd_gpk).
**Note:** When **Cross-account/Cross-region** is set to **Yes**, **Service Endpoint** is displayed.

|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
        |**Alibaba Cloud Account**|The ID of the Alibaba Cloud account that you can use to log on to Function Compute. In the Function Compute console, you can view the ID of the Alibaba Cloud account in the **Common Info** section on the **Overview** page. **Note:** When **Cross-account/Cross-region** is set to **Yes**, **Alibaba Cloud Account** is displayed.

|188\*\*\*|
        |**RAM Role**|The name of the RAM role that Message Queue for Apache Kafka is authorized to assume to access Function Compute.         -   If you do not need to use a different Alibaba Cloud account, you must create a RAM role and grant the RAM role the required permissions. Then, enter the name of the RAM role. For more information, see [Create a custom policy](#section_3wj_qkk_gwt), [Create a RAM role](#section_24p_yc7_s0d), and [Grant permissions](#section_co0_y32_ams).
        -   If you want to use a different Alibaba Cloud account, you must create a RAM role by using the Alibaba Cloud account within which you want to use Function Compute. Then, grant the RAM role the required permissions and enter the name of the RAM role. For more information, see [Create a custom policy](#section_3wj_qkk_gwt), [Create a RAM role](#section_24p_yc7_s0d), and [Grant permissions](#section_co0_y32_ams).
**Note:** When **Cross-account/Cross-region** is set to **Yes**, **RAM Role** is displayed.

|AliyunKafkaConnectorRole|
        |**Service Name**|The name of the service in Function Compute.|guide-hello\_world|
        |**Function Name**|The name of the function under the service in Function Compute.|hello\_world|
        |**Service Version or Alias**|The version or alias of the service in Function Compute.|LATEST|
        |**Transmission Mode**|The mode in which messages are sent. Valid values:         -   **Async**: This value is recommended.
        -   **Sync**: This value is not recommended. In synchronous mode, if Function Compute takes a long time to process messages, Message Queue for Apache Kafka also takes a long time to process messages. If Function Compute takes more than 5 minutes to process a batch of messages, the Message Queue for Apache Kafka client rebalances the traffic.
|Asynchronous|
        |**Data Size**|The number of messages that can be sent at a time. Default value: 20. The connector aggregates the messages to be sent at a time based on the maximum number of messages and the maximum allowed message size in a request. The maximum allowed message size cannot exceed 6 MB in synchronous mode or 128 KB in asynchronous mode. Assume that messages are sent in asynchronous mode and up to 20 messages can be sent at a time. If you want to send 18 messages, 17 messages have a total size of 127 KB, and one message is 200 KB in size. In this case, the connector sends the 17 messages as a single batch, and then separately sends the message whose size is more than 128 KB. **Note:** If you set key to null when you send a message, the request does not contain key. If you set value to null, the request does not contain value.

        -   If the size of messages in a batch do not exceed the maximum allowed message size in a request, the request contains the content of the messages. The following code provides a sample request:

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

        -   If a single message exceeds the maximum allowed message size in a request, the request does not contain the content of the message. The following code provides a sample request:

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

**Note:** To obtain the content of the message, you must pull the message by using its offset.

|50|
        |**Retries**|The number of retries allowed after a message fails to be sent. Default value: 2. Valid values: 1 to 3. In some cases where a message fails to be sent, retries are not supported. The following information describes the types of [Error codes]() and whether they support retries:        -   4XX: does not support a retry except for 429.
        -   5XX: supports a retry.
**Note:** The connector calls the [InvokeFunction]() operation to send a message to Function Compute.

|2|
        |**Failure Handling Policy**|Specifies whether to continue subscribing to the partition where an error occurs after the message fails to be sent. Valid values:        -   log: retains the subscription to the partition where an error occurs and prints the logs.
        -   fail: stops the subscription to the partition where an error occurs and prints the logs.
**Note:**

        -   For more information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
        -   For information about how to troubleshoot errors based on error codes, see [Error codes]().
        -   To resume the subscription to the partition where an error occurs,[submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to Message Queue for Apache Kafka Customer Services.
|log|

    4.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

8.  In the **Create Connector** panel, click **Deploy**.

    To configure Function Compute resources, click **Configure Function** to go to the Function Compute console and complete the operation.


## Send a test message

After you deploy the Function Compute sink connector, you can send a message to the data source topic in Message Queue for Apache Kafka to test whether the message can be synchronized to Function Compute.

1.  On the **Connector \(Public Preview\)** page, find the connector that you created, and click **Test** in the **Actions** column.

2.  On the **Topics** page, select your instance, find the **fc-test-input** topic, click the More icon in the **Actions** column, and then select **Send Message**.

3.  In the **Send Message** panel, set the parameters used to send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## View function logs

After you send a message to the data source topic in Message Queue for Apache Kafka, you can view the function logs to check whether the message is received. For more information, see [Configure Log Service resources and view function execution logs]().

The test message that you sent appears in the logs.

![fc LOG](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2228646061/p127831.png)

