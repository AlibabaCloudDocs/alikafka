---
keyword: [kafka, connector, ]
---

# Delete a connector

Message Queue for Apache Kafka limits the number of connectors for each instance. If you no longer need a connector, you can delete it in the Message Queue for Apache Kafka console.

One of the connectors that are described in the following topics is created:

-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)
-   [Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an OSS sink connector.md)
-   [Create an Elasticsearch sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an Elasticsearch sink connector.md)
-   [Create a MySQL source connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MySQL source connector.md)

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the left-side navigation pane, click **Connector**.

6.  On the **Connector \(Public Preview\)** page, find the connector that you want to delete, click the ![icon_more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6022597161/p185678.png) icon in the **Actions** column, and then select **Delete**.

7.  In the **Delete** message, click **OK**.

    **Note:** When you delete a connector, the system deletes the five topics and two consumer groups that the connector requires, regardless of whether they were automatically or manually created.


