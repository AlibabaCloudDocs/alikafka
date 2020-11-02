---
keyword: [kafka, connector, ]
---

# Delete a connector

Message Queue for Apache Kafka limits the number of connectors for each instance. If you no longer need a connector, you can delete it in the Message Queue for Apache Kafka console.

You have created a connector. For more information, see the following topics:

-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)
-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Connector**.

4.  On the **Connector** page, select an instance, find the connector that you want to delete, click More, and then select Delete in the **Actions** column.

5.  In the **Delete** message, click **OK**.

    **Note:** When you delete a connector, the system also deletes the five topics and two consumer groups that the connector requires, regardless of whether they were automatically or manually created.


