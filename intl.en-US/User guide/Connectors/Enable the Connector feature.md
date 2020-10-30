---
keyword: [kafka, connector, enable]
---

# Enable the Connector feature

This topic describes how to enable the Connector feature for your Message Queue for Apache Kafka instance.

You have purchased and deployed a Message Queue for Apache Kafka instance. The instance meets the requirements listed in the following table.

|Item|Description|
|----|-----------|
|Running status|Running|
|Version|The version of the Message Queue for Apache Kafka instance meets one of the following requirements:-   The major version is 0.10.2, and the minor version is the latest.
-   The major version is 2.2.0. |

**Note:** You can view the running status and version of an instance in the **Basic Information** section on the **Instance Details** page of the Message Queue for Apache Kafka console.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Connector**.

4.  On the **Connector** page, select an instance and click **Enable Connector**.

5.  In the **Note** message, click **OK**.


After the Connector feature is enabled for your Message Queue for Apache Kafka instance, you can create a Function Compute or MaxCompute sink connector to synchronize data from your Message Queue for Apache Kafka instance to Function Compute or MaxCompute.

-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)

