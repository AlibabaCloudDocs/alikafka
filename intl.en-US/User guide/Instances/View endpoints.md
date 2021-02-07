---
keyword: [kafka, endpoint, send and subscribe to messages]
---

# View endpoints

To use the SDK to connect to a Message Queue for Apache Kafka instance and send and subscribe to messages, you must configure an endpoint based on the network type of the instance. You can view the endpoint of your instance in the Message Queue for Apache Kafka console.

Message Queue for Apache Kafka provides the following types of endpoints:

-   Default endpoint: It is used to send and subscribe to messages in a virtual private cloud \(VPC\), but does not support Simple Authentication and Security Layer \(SASL\) authentication.
-   SASL endpoint: It is used to send and subscribe to messages in a VPC, and supports SASL authentication.
-   Secure Sockets Layer \(SSL\) endpoint: It is used to send and subscribe to messages over the Internet, and supports SASL authentication.

For more information about the differences among these endpoints, see [Comparison among endpoints](/intl.en-US/Introduction/Comparison between endpoints.md).

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your resource resides.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of your instance.

5.  On the **Instance Details** page, view the endpoint of the instance in the **Basic Information** section.

    **Note:**

    -   If the value of Instance Type is VPC Instance, only Default Endpoint is displayed.
    -   If the value of Instance Type is Public Network/VPC Instance, both Default Endpoint and SSL Endpoint are displayed.
    -   By default, SASL endpoints are not configured for instances. Therefore, by default, SASL endpoints are not displayed. To use SASL endpoints, you must apply for authorization. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

After you obtain the endpoint of your Message Queue for Apache Kafka instance, you can use the SDK to connect to the instance and send and subscribe to messages. For more information, see [SDK overview](/intl.en-US/SDK reference/SDK overview.md).

