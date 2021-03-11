# Create a connector to synchronize data to OSS

This topic describes how to synchronize data from a data source topic of a Message Queue for Apache Kafka instance to an Object Storage Service \(OSS\) bucket.

## Prerequisites

**Note:** To synchronize data from Message Queue for Apache Kafka to OSS, the Message Queue for Apache Kafka instance that contains the data source topic and the destination OSS bucket must be in the same region. Message Queue for Apache Kafka first synchronizes the data to Function Compute. Then, Function Compute synchronizes the data to OSS.

Before you start, make sure that the following requirements are met:

-   The connector feature for your Message Queue for Apache Kafka instance is enabled. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
-   A data source topic for the Message Queue for Apache Kafka instance is created. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).
-   An OSS bucket is created in the [OSS console](https://oss.console.aliyun.com/bucket). For more information, see [Create buckets](https://help.aliyun.com/document_detail/31885.html?spm=a2c4g.11174283.6.614.1bf37da24aeBfe).
-   Function Compute is activated. For more information, see [Create a function in the Function Compute console]().

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, click **Create Connector**.

7.  Perform the following steps based on the **Create Connector** wizard:

    1.  In the **Enter Basic Information** step, enter a connector name in the **Connector Name** field, select **Message Queue for Apache Kafka** from the **Dump Path** drop-down list and **Object Storage Service** from the **Dump To** drop-down list, and then click **Next**.

        **Note:** The **Authorize to Create Service Linked Role** option is automatically selected, which means that Message Queue for Apache Kafka will create a service-lined role based on the following rules:

        -   If you have not created a service-linked role, Message Queue for Apache Kafka automatically creates one. The service-linked role enables you to synchronize data from Message Queue for Apache Kafka to OSS.
        -   If you have created a service-linked role, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md).

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Connector Name|The name of the connector. Take note of the following rules when you specify a connector name:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The connector name must be unique for the same Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the format of connect-task name. If you have not created such a consumer group, the system automatically creates a consumer group for you.

|kafka-oss-sink|
        |Task Type|The type of the data synchronization task of the connector. In this example, the task synchronizes data from Message Queue for Apache Kafkato OSS. For more information about task types, see [Types of connectors](/intl.en-US/User guide/Connectors/Overview.md).|KAFKA2OSS|

    2.  In the **Configure Source Instance** step, select the data source topic from the **Data Source Topic** drop-down list, select a consumer offset from the **Consumer Offset** drop-down list, select the number of concurrent consumption threads from the **Consumer Thread Concurrency** drop-down list, and then click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Data Source Topic|The name of the topic from which data is to be synchronized.|oss-test-input|
        |Consumer Offset|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |Consumer Thread Concurrency|The number of concurrent consumption threads to synchronize data from the data source topic. Default value: 6. Valid values:        -   6
        -   12
|6|

    3.  In the **Configure Destination Instance Configure Runtime Environment** section, set the parameters as required.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |Bucket Name|The name of the destination OSS bucket.|bucket\_test|
        |AccessKey ID|The AccessKey ID of your Alibaba Cloud account.|LTAI4GG2RGAjppjK\*\*\*\*\*\*\*\*|
        |AccessKey Secret|The AccessKey secret of your Alibaba Cloud account.|WbGPVb5rrecVw3SQvEPw6R\*\*\*\*\*\*\*\*|

        **Note:**

        When Message Queue for Apache Kafka creates the data synchronization task, the AccessKey ID and AccessKey Secret parameters are passed to Function Compute as environment variables. Message Queue for Apache Kafka does not store the AccessKey ID and AccessKey secret of your Alibaba Cloud account.

    4.  In the **Configure Destination Instance Configure Runtime Environment** section, set the parameters as required and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |VPC ID|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to change the value.|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|The ID of the vSwitch where the data synchronization task runs. The vSwitch must be deployed in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you have specified for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |Failure Handling Policy|The solution to message delivery failures. Default value: log. Valid values:        -   log: Retain the subscription to the partition where an error occurs and query the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

            -   For more information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to troubleshoot errors based on error codes, see [Error codes]().
        -   fail: Stop the subscription to the partition where an error occurs and query the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

            -   For more information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to troubleshoot errors based on error codes, see [Error codes]().
            -   To resume the subscription to the partition where an error occurs, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to request technical support of Message Queue for Apache Kafka.
|log|
        |Create Resource|The mode in which to create the consumer group and topics used for data synchronization. Valid values: **Automatically** and **Manually**.|Automatically|
        |Connector consumer group|The name of the consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-oss-sink|
        |Task site Topic|The topic that is used to store consumer offsets.        -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. The value must be greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-offset-kafka-oss-sink|
        |Task configuration Topic|The topic that is used to store task configurations.        -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the value to 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-config-kafka-oss-sink|
        |Task status Topic|The topic that is used to store task statuses.        -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-status-kafka-oss-sink|
        |Abnormal Data Topic|The topic that is used to store the abnormal data of the connector. To save topic resources, this topic and the dead-letter queue topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-oss-sink|
        |Dead letter queue Topic|The topic that is used to store the abnormal data of the connector framework. To save topic resources, this topic and the abnormal data topic can be the same topic.        -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Valid values: Local Storage and Cloud Storage.
|connect-error-kafka-oss-sink|

    5.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

8.  On the **Create Connector** page, click **Deploy** in the Actions column of the created connector.


## Send test messages

After you deploy the connector, you can send messages to the data source topic in Message Queue for Apache Kafka to check whether the data can be synchronized to OSS.

1.  On the **Connector \(Public Preview\)** page, find the created connection and click **Test** in the **Actions** column.

2.  On the **Topics** page, find the data source topic, click the More icon in the **Actions** column, and then select **Send Message**.

3.  In the **Send Message** panel, set the parameters used to send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## Verify the test result

After you send test messages to the data source topic in Message Queue for Apache Kafka, you can check the destination OSS bucket to verify the test result. For more information, see [Overview](https://help.aliyun.com/document_detail/31908.html?spm=a2c4g.11186623.6.1828.183d62e7KAJ360).

If new objects are generated in the OSS bucket, the data has been synchronized to OSS, as shown in the following figure.

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

