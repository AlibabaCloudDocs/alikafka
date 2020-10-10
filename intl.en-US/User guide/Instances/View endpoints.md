---
keyword: [Kafka, endpoint, message sending and subscription]
---

# View endpoints

To send and subscribe to Message Queue for Apache Kafka messages by using the software development kit \(SDK\), you need to configure the endpoint according to the network type of the instance. You can view the endpoint of your instance in the Message Queue for Apache Kafka console.

## Background

Message Queue for Apache Kafka provides the following types of endpoints:

-   Default Endpoint: is applicable to message sending and subscription in a Virtual Private Cloud \(VPC\) environment.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com) and select a region in the top navigation bar.
2.  In the left-side navigation pane, click **Instances**.
3.  On the **Instance Details** page, click the target instance.
4.  Check the endpoint information in the **Basic Information** section.

    ![view_endpoint](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5840549951/p93877.png)

    **Note:**

    -   If the value of Instance Type is VPC Instance, only Default Endpoint is displayed.

## More information

-   -   [Access from VPC](/intl.en-US/Quick-start/Step 4: Use the SDK to send and subscribe to messages/Access from VPC.md)

