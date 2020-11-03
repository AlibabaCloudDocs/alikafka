---
keyword: [kafka, connector, enable]
---

# Enable the connector feature

This topic describes how to enable the connector feature for your Message Queue for Apache Kafka instance.

A Message Queue for Apache Kafka instance is purchased and deployed, and the following conditions are met by the instance.

|Item|Description|
|----|-----------|
|Status|The status of the instance must be Running.|
|Version|The version of the Message Queue for Apache Kafka instance must be one of the following:-   The major version is 0.10.2, with the latest minor version.
-   The major version is 2.2.0. |

**Note:** In the Message Queue for Apache Kafka console, you can view the running status and version of the instance in the **Basic Information** section on the **Instance Details** page.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Connector**.

4.  On the **Connector** page, click an instance and click **Enable Connector**.

5.  In the **Note** dialog box, click **OK**.


After the connector feature is enabled for your Message Queue for Apache Kafka instance, you can create a Function Compute or MaxCompute sink connector to synchronize data from your Message Queue for Apache Kafka instance to Function Compute or MaxCompute.

-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)

