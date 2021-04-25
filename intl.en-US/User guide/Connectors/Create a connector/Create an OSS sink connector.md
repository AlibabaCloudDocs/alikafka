# Create an OSS sink connector

This topic describes how to create an OSS sink connector to synchronize data from a topic in your Message Queue for Apache Kafka instance to Object Storage Service \(OSS\).

## Prerequisites

Before you export data, make sure that the following operations are completed:

-   The connector feature is enabled for the Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
-   A data source topic is created in the Message Queue for Apache Kafka instance. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).
-   Buckets are created in the [OSS console](https://oss.console.aliyun.com/bucket). For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).
-   Function Compute is activated. For more information, see [Create a function in the Function Compute console]().

## Considerations

-   To synchronize data from Message Queue for Apache Kafka to OSS, the Message Queue for Apache Kafka instance that contains the data source topic and the destination OSS bucket must be in the same region. Message Queue for Apache Kafka first synchronizes the data to Function Compute. Then, Function Compute synchronizes the data to OSS. For information about the limits on connectors, see [Limits](/intl.en-US/User guide/Connectors/Overview.md).
-   OSS sink connectors are provided based on Function Compute. Function Compute provides you with a free quota. If your usage exceeds the free quota, you are charged for the excess based on the billing rules of Function Compute. For information about the billing details, see [Billing]().
-   Function Compute allows you to query the logs of function calls. For more information, see [Configure Log Service resources and view function execution logs]().
-   For message transfer, Message Queue for Apache Kafka serializes data into UTF-8-encoded strings and does not support the BINARY data type.

## Create and deploy an OSS sink connector

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, click **Create Connector**.

7.  In the **Create Connector** wizard, perform the following steps:

    1.  In the **Enter Basic Information** step, set the parameters that are described in the following table, and click **Next**.

        **Note:** By default, the **Authorize to Create Service Linked Role** check box is selected. This means that Message Queue for Apache Kafka will create a service-lined role based on the following rules:

        -   If no service-linked role is created, Message Queue for Apache Kafka automatically creates a service-linked role for you to use the OSS sink connector to synchronize data from Message Queue for Apache Kafka to OSS.
        -   If you have created a service-linked role, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md).

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Connector Name**|The name of the connector. Take note of the following rules when you specify a connector name:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The connector name must be unique within a Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the connect-Task name format. If you have not manually created such a consumer group, the system automatically creates a consumer group for you.

|kafka-oss-sink|
        |**Dump Path**|The source and destination of data transfer. Select a data source from the first drop-down list and a destination from the second drop-down list.|**Message Queue for Apache Kafka** Dump To **Object Storage Service**|

    2.  In the **Configure Source Instance** step, set the parameters that are described in the following table, and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Data Source Topic**|The name of the topic from which data is to be synchronized.|oss-test-input|
        |**Consumer Offset**|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |**Consumer Thread Concurrency**|The number of concurrent consumer threads to synchronize data from the data source topic. Default value: 6. Valid values:        -   6
        -   12
|6|

    3.  In the **Configure Destination Instance Configure Runtime Environment** step, set the parameters related to the destination OSS bucket.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Bucket Name**|The name of the destination OSS bucket.|bucket\_test|
        |**AccessKey ID**|The AccessKey ID of your Alibaba Cloud account.|LTAI4GG2RGAjppjK\*\*\*\*\*\*\*\*|
        |**AccessKey Secret**|The AccessKey secret of your Alibaba Cloud account.|WbGPVb5rrecVw3SQvEPw6R\*\*\*\*\*\*\*\*|

        Make sure that your Alibaba Cloud account is granted at least the following permissions:

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "oss:GetObject",
                        "oss:PutObject"
                    ],
                    "Resource": "*",
                    "Effect": "Allow"
                }
            ]
        }
        ```

        **Note:**

        When Message Queue for Apache Kafka creates the data synchronization task, the AccessKey ID and AccessKey Secret parameters are passed to Function Compute as environment variables. Message Queue for Apache Kafka does not store the AccessKey ID or AccessKey secret of your Alibaba Cloud account.

    4.  In the **Configure Destination Instance Configure Runtime Environment** step, set the parameters that are described in the following table, and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**VPC ID**|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to enter a value.|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch**|The ID of the vSwitch based on which the data synchronization task runs. The vSwitch must be deployed in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you specify for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |**Failure Handling Policy**|The error handling policy for a message that fails to be sent. Default value: log. Valid values:        -   log: retains the subscription to the partition where an error occurs and prints the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.
        -   fail: stops the subscription to the partition where an error occurs and prints the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

            -   For information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For information about how to troubleshoot errors based on error codes, see [Error codes]().
            -   To resume the subscription to the partition where an error occurs, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to Message Queue for Apache Kafka Customer Services.
|log|
        |**Create Resource**|The mode in which to create the consumer group and topics used for data synchronization. Valid values: **Automatically** and **Manually**. If you select Manually, enter resource names.|Automatically|
        |**Connector consumer group**|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-oss-sink|
        |**Task site Topic**|The topic that is used to store consumer offsets.         -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. Set it to a value greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-offset-kafka-oss-sink|
        |**Task configuration Topic**|The topic that is used to store task configurations.         -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the parameter to 1.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-config-kafka-oss-sink|
        |**Task status Topic**|The topic that is used to store task status.         -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Set the parameter to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the parameter to compact.
|connect-status-kafka-oss-sink|
        |**Abnormal Data Topic**|The topic that is used to store the abnormal data of the connector. To save topic resources, you can create a topic as both the **Dead letter queue Topic** and the abnormal data topic.         -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-oss-sink|
        |**Dead letter queue Topic**|The topic that is used to store the abnormal data of the connector framework. To save topic resources, you can create a topic as both the dead-letter queue topic and the **Abnormal Data Topic** topic.         -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the parameter to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-oss-sink|

    5.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

8.  In the **Create Connector** panel, click **Deploy**.


## Send a test message

After you deploy the OSS sink connector, you can send a message to the data source topic in Message Queue for Apache Kafka to test whether the message can be synchronized to OSS.

1.  On the **Connector \(Public Preview\)** page, find the connector that you created, and click **Test** in the **Actions** column.

2.  On the **Topics** page, select your instance, find the data source topic, click the More icon in the **Actions** column, and then select **Send Message**.

3.  In the **Send Message** panel, set the parameters used to send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## Verify the results

After you send a test message to the data source topic in Message Queue for Apache Kafka, you can check whether the message is synchronized to OSS on the Files page of the destination OSS bucket in the OSS console. For more information, see [Overview](/intl.en-US/Console User Guide/Upload, download, and manage objects/Overview.md).

If new objects are generated in the OSS bucket, the data is synchronized to OSS, as shown in the following figure.

![files](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8852545161/p243372.png)

The data that is synchronized from Message Queue for Apache Kafka to OSS is in the following format:

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

## Perform other operations

You can configure the Function Compute resources that are required by the OSS sink connector based on actual needs.

1.  On the **Connector \(Public Preview\)** page, find the connector that you created, and click **Configure Function** in the **Actions** column.

    You are redirected to the Function Compute console, where you can configure the resources as required.


