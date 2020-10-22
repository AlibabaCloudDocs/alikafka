---
keyword: [kafka, endpoint, send and subscribe to messages]
---

# View endpoints

To use the SDK to connect to a Message Queue for Apache Kafka instance to send and subscribe to messages, you must configure an endpoint based on the network type of the instance. You can view the endpoint of your instance in the Message Queue for Apache Kafka console.

Message Queue for Apache Kafka provides the following types of endpoints:

-   Default endpoint: used for sending and subscribing to messages in a virtual private cloud \(VPC\), but does not support Simple Authentication and Security Layer \(SASL\) authentication.
-   SASL endpoint: used for sending and subscribing to messages in a VPC, and supports SASL authentication.

For more information about the differences between these endpoints, see [t1884077.md\#](/intl.en-US/Introduction/Endpoint comparison.md).

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instance Details** page, click an instance, and then view the endpoint in the **Basic Information** section.

    **Note:**

    -   If the value of Cluster Type is VPC Instance, only Default Endpoint is displayed.
    -   By default, SASL endpoints are not configured for instances. Therefore, SASL endpoints are not displayed by default. To use SASL endpoints, you must apply for authorization. For more information, see [t1884061.md\#](/intl.en-US/Access control/Authorize SASL users.md).

After you obtain the endpoint of an instance, you can use the SDK to access the Message Queue for Apache Kafka instance to send and subscribe to messages. For more information, see [t998844.md\#](/intl.en-US/SDK reference/Overview.md).

