---
keyword: [connector, kafka, modify description]
---

# Modify connector configurations

After you create a Function Compute sink connector, you can modify its configurations in the Message Queue for Apache Kafka console.

[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector \(Public Preview\)**.

6.  On the **Connector \(Public Preview\)** page, find the Function Compute sink connector whose configurations you want to modify, click the ![icon_more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6022597161/p185678.png) icon in the **Actions** column, and then select **Modify Configuration**.

7.  In the **Modify Connector** panel, modify the following configurations as needed, and then click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |Consumer Thread Concurrency|The number of concurrent consumption threads to synchronize data from the data source topic. Default value: 3. Valid values:    -   3
    -   6
    -   9
    -   12 |
    |Retries|The error handling policy for a message that fails to be sent. Default value: log. Valid values:    -   log: Retain the subscription to the partition where an error occurs and query the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

        -   For information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
        -   For information about how to troubleshoot errors based on error codes, see [Error codes]().
    -   fail: Stop the subscription to the partition where an error occurs and query the logs. After an error occurs, you can view the error in the connector logs. Then, you can troubleshoot the error based on the returned error code.

**Note:**

        -   For information about how to view the connector logs, see [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
        -   For information about how to troubleshoot errors based on error codes, see [Error codes]().
        -   To resume the subscription to the partition where an error occurs, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352) to seek technical support for Message Queue for Apache Kafka. |
    |Retries|The number of retries allowed after a message fails to be sent. Default value: 2. Valid values: 1 to 3. In some cases where a message fails to be sent, retries are not supported. The following list describes the types of error codes and whether they support retries:    -   4XX: does not support a retry except for 429.
    -   5XX: supports a retry.
**Note:**

    -   For more information about error codes, see [Error codes]().
    -   The connector calls the InvokeFunction operation to send messages to Function Compute. For more information about the InvokeFunction operation, see [List of operations by function](). |


After the modification is complete, you can view the updated connector configurations in the **View task configuration** panel.

