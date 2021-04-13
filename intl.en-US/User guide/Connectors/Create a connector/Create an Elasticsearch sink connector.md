# Create an Elasticsearch sink connector

This topic describes how to create an Elasticsearch sink connector to export data from a topic in your Message Queue for Apache Kafka instance to an index of your Elasticsearch instance.

Before you start, make sure that the following operations are complete:

-   Enable the connector feature for your Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
-   Create topics in your Message Queue for Apache Kafka instance. For more information, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).
-   Create an instance and index in the [Elasticsearch console](https://elasticsearch.console.aliyun.com/). For more information, see [Quick start](/intl.en-US/Elasticsearch Instances Management/Overview.md).

    **Note:** The version of the Elasticsearch client used by Function Compute is 7.7.0. To maintain compatibility, create an Elasticsearch instance of version 7.0 or later.

-   [Activate Function Compute]().

## Usage notes

-   To export data from Message Queue for Apache Kafka to Elasticsearch, the Message Queue for Apache Kafka instance that contains the data source topic and the Elasticsearch instance must be in the same region. Message Queue for Apache Kafka first exports the data to Function Compute. Then, Function Compute exports the data to Elasticsearch. For information about limits on connectors, see [Limits](/intl.en-US/User guide/Connectors/Overview.md).
-   Elasticsearch sink connectors are provided based on Function Compute. Function Compute provides you with a free quota. If your usage exceeds the free quota, you are charged for the excess based on the billing rules of Function Compute. For more information, see [Billing]().
-   Function Compute allows you to query the logs of function calls. For more information, see [Configure Log Service resources and view function execution logs]().
-   Message Queue for Apache Kafka serializes messages into UTF-8-encoded strings for message transfer. Message Queue for Apache Kafka does not support the BINARY data type.

## Create and deploy an Elasticsearch sink connector

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, click **Create Connector**.

7.  In the **Create Connector** wizard, perform the following steps:

    1.  In the **Enter Basic Information** step, set **Connector Name**, configure a dump path from **Message Queue for Apache Kafka** to **Elasticsearch** for **Dump Path**, and then click **Next**.

        **Note:** By default, **Authorize to Create Service Linked Role** is selected. This means that Message Queue for Apache Kafka will create a service-linked role based on the following rules:

        -   If you have not created a service-linked role, Message Queue for Apache Kafka automatically creates a service-linked role for you to export data from Message Queue for Apache Kafka to Elasticsearch.
        -   If you have created a service-linked role, Message Queue for Apache Kafka does not create it again.
        For more information about service-linked roles, see [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md).

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Connector Name**|The name of the connector. Take note of the following rules when you specify a connector name:        -   The connector name must be 1 to 48 characters in length. It can contain digits, lowercase letters, and hyphens \(-\), but cannot start with a hyphen \(-\).
        -   The connector name must be unique within the Message Queue for Apache Kafka instance.
The data synchronization task of the connector must use a consumer group that is named in the connect-Task name format. If you have not created such a consumer group, the system automatically creates a consumer group for you.

|kafka-elasticsearch-sink|
        |**Dump Path**|The source and destination of data transfer. Select a data source from the first drop-down list and a destination from the second drop-down list.|Dump from **Message Queue for Apache Kafka** to **Elasticsearch**|

    2.  In the **Configure Source Instance** step, set the parameters that are described in the following table, and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**Data Source Topic**|The name of the topic from which data is to be synchronized.|elasticsearch-test-input|
        |**Consumer Offset**|The offset where consumption starts. Valid values:         -   latest: Consumption starts from the latest offset.
        -   earliest: Consumption starts from the initial offset.
|latest|
        |**Consumer Thread Concurrency**|The number of concurrent consumer threads to synchronize data from the data source topic. Default value: 6. Valid values:        -   6
        -   12
|6|

    3.  In the **Configure Destination Instance Configure Runtime Environment** step, set the parameters related to the destination Elasticsearch instance.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        | |The ID of the Elasticsearch instance.|es-cn-oew1o67x0000\*\*\*\*|
        | |The public or private endpoint of the Elasticsearch instance. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|es-cn-oew1o67x0000\*\*\*\*.elasticsearch.aliyuncs.com|
        | |The public or private port used to access the Elasticsearch instance. Valid values:        -   9200: for HTTP and HTTPS
        -   9300: for TCP
For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

|9300|
        | |The username that is used to log on to the Kibana console. Default value: elastic. You can also customize the username. For more information, see [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).|elastic|
        | |The password used to log on to the Kibana console. The password of the elastic user is specified when you create the Elasticsearch instance. If you forget the password, you can reset it. For more information about how to reset the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|\*\*\*\*\*\*\*\*|
        | |The name of the Elasticsearch index.|elastic\_test|

        **Note:**

        -   The username and password will be used to initialize Elasticsearch objects. To ship messages by using bulk, make sure that the account has the write permission on the index.
        -   The username and password are passed to functions of Function Compute as environment variables when Message Queue for Apache Kafka creates a synchronization task. After the synchronization task is created, Message Queue for Apache Kafka does not save the username or password.
    4.  In the **Configure Destination Instance Configure Runtime Environment** section, set the parameters as required and click **Next**.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |**VPC ID**|The ID of the virtual private cloud \(VPC\) where the data synchronization task runs. The default value is the ID of the VPC where the Message Queue for Apache Kafka instance is deployed. You do not need to change the value.|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch**|The ID of the vSwitch based on which the data synchronization task runs. The vSwitch must be deployed in the same VPC as the Message Queue for Apache Kafka instance. The default value is the ID of the vSwitch that you specify for the Message Queue for Apache Kafka instance.|vsw-bp1d2jgg81\*\*\*|
        |**Failure Handling Policy**|The error handling policy for a message that fails to be sent. Default value: log. Valid values: log and fail.        -   log: retains the subscription to the partition where an error occurs and prints the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

            -   For more information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to troubleshoot errors based on error codes, see [Error codes]().
        -   fail: stops the subscription to the partition where an error occurs and prints the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

            -   For more information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
            -   For more information about how to troubleshoot errors based on error codes, see [Error codes]().
            -   To resume the subscription to the partition where an error occurs,[submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to Message Queue for Apache Kafka Customer Services.
|log|
        |**Create Resource**|The mode in which to create the consumer group and topics used for data synchronization. Valid values: **Automatically** and **Manually**. If you select Manually, enter resource names.|Automatically|
        |**Connector consumer group**|The consumer group that is used by the connector. We recommend that you start the name of this consumer group with connect-cluster.|connect-cluster-kafka-elasticsearch-sink|
        |**Task site Topic**|The topic that is used to store consumer offsets.         -   Topic: the name of the topic. We recommend that you start the name with connect-offset.
        -   Partitions: the number of partitions in the topic. Set it to a value greater than 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-offset-kafka-elasticsearch-sink|
        |**Task configuration Topic**|The topic that is used to store task configurations.         -   Topic: the name of the topic. We recommend that you start the name with connect-config.
        -   Partitions: the number of partitions in the topic. Set the value to 1.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-config-kafka-elasticsearch-sink|
        |**Task status Topic**|The topic that is used to store task status.         -   Topic: the name of the topic. We recommend that you start the name with connect-status.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage.
        -   cleanup.policy: the log cleanup policy for the topic. Set the value to compact.
|connect-status-kafka-elasticsearch-sink|
        |**Abnormal Data Topic**|The topic that is used to store the error data of the sink connector. To save topic resources, you can create a topic as both the **Dead letter queue Topic** and the error data topic.         -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage or Cloud Storage.
|connect-error-kafka-elasticsearch-sink|
        |**Dead letter queue Topic**|The topic that is used to store the error data of the connector framework. To save topic resources, you can create a topic as both the dead-letter queue topic and the **Abnormal Data Topic**.         -   Topic: the name of the topic. We recommend that you start the name with connect-error.
        -   Partitions: the number of partitions in the topic. We recommend that you set the value to 6.
        -   Storage Engine: the storage engine of the topic. Set the value to Local Storage or Cloud Storage.
|connect-error-kafka-elasticsearch-sink|

    5.  In the **Preview/Submit** step, confirm the configurations of the connector and click **Submit**.

8.  In the connector list, find the connector that you created, and click **Actions** in the **Deploy** column.

    If the connector status changes to **Running**, the connector is deployed.


## Create Function Compute resources

After you create and deploy the Elasticsearch sink connector in the Message Queue for Apache Kafka console, Function Compute automatically creates a service for the connector and names it in the `kafka-service-<connector_name>-<random string>` format.

1.  On the **Connector \(Public Preview\)** page, find the connector that you created, click the More icon in the **Actions** column, and then select **Configure Function**.

    The page is redirected to the Function Compute console.

2.  In the Function Compute console, find the automatically created service and configure a VPC and vSwitch for the service. Make sure that the VPC and vSwitch are the same as those of your Elasticsearch instance. For more information, see [Modify a service]().


## Send a test message

You can send a message to the data source topic in Message Queue for Apache Kafka to test whether the data can be exported to Elasticsearch.

1.  On the **Connector \(Public Preview\)** page, find the connector that you created, and click **Test** in the **Actions** column.

2.  On the **Topics** page, select your instance, find the data source topic, click the More icon in the **Actions** column, and then select **Send Message**.

3.  In the **Send Message** panel, set the parameters used to send a test message.

    1.  In the **Partitions** field, enter 0.

    2.  In the **Message Key** field, enter 1.

    3.  In the **Message Value** field, enter 1.

    4.  Click **Send**.


## Verify the result

After you send a message to the data source topic in your Message Queue for Apache Kafka instance, [log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) and run the `GET /<index_name>/_search` command to view the Elasticsearch index and verify whether data is exported.

The following code shows an example of the data exported from Message Queue for Apache Kafka to Elasticsearch.

```
{
  "took" : 8,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "product_****",
        "_type" : "_doc",
        "_id" : "TX3TZHgBfHNEDGoZ****",
        "_score" : 1.0,
        "_source" : {
          "msg_body" : {
            "key" : "test",
            "offset" : 2,
            "overflowFlag" : false,
            "partition" : 2,
            "timestamp" : 1616599282417,
            "topic" : "dv****",
            "value" : "test1",
            "valueSize" : 8
          },
          "doc_as_upsert" : true
        }
      }
    ]
  }
}
}
```

