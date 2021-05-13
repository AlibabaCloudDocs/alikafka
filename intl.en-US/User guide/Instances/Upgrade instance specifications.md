---
keyword: [kafka, specification, upgrade]
---

# Upgrade instance specifications

This topic describes how to upgrade the specifications of an instance in the Message Queue for Apache Kafka console. You can upgrade the instance edition, traffic specification, disk capacity, and topic specification.

## Scenarios

-   The disk usage of your Message Queue for Apache Kafka instance is high and will affect service running.
-   The traffic of your Message Queue for Apache Kafka instance continuously exceeds the traffic specification that you purchase. As a result, the instance fails to meet your business needs.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  On the **Instance Details** page, click **Upgrade Instance** in the upper-right corner of the **Basic Information** section.

6.  In the **Instance Upgrade Risks** dialog box, read the risks of upgrading an instance, select **I am aware of the service unavailability risk caused by restart of servers in the cluster.**, and then click **I am aware of the risks.**.

7.  On the **Upgrade/Downgrade** page, change the specifications.

    |Parameter|Description|
    |---------|-----------|
    |Edition|    -   You can upgrade an instance from the Standard Edition to the Professional Edition.
    -   You can upgrade the traffic specification of a Professional Edition instance to a higher specification.
    -   You cannot downgrade an instance from the Professional Edition to the Standard Edition.
For more information about editions and pricing of Message Queue for Apache Kafka instances, see [Billing](/intl.en-US/Pricing/Billing.md). |
    |Internet Traffic|Public traffic is divided into read traffic and write traffic. The maximum read traffic and maximum write traffic provided by Message Queue for Apache Kafka are the same. Select a bandwidth based on your peak read or write traffic, whichever is higher. This billable item applies only to instances of the Internet and VPC type.|
    |Traffic Specification|When you upgrade the traffic specification of your instance, the following limits apply:    -   Edition
        -   Standard Edition: You can select a traffic specification that supports the maximum traffic of 120 MB/s. If you need a traffic specification that supports the maximum traffic of more than 120 MB/s, upgrade your instance to the Professional Edition. Then, upgrade the traffic specification.
        -   Professional Edition \(High Write\): You can select a traffic specification that supports the maximum traffic of 2,000 MB/s.
        -   Professional Edition \(High Read\): You can select a traffic specification that supports the maximum read traffic of 150 MB/s and the maximum write traffic of 30 MB/s.
    -   Disk type
        -   Ultra disk: If the maximum traffic supported by the traffic specification that you want to select exceeds 120 MB/s, the corresponding cluster will be scaled out. After the upgrade is complete, you must rebalance topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md).
        -   SSD: If the maximum traffic supported by the traffic specification that you want to select exceeds 300 MB/s, the corresponding cluster will be scaled out. After the upgrade is complete, you must rebalance topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md). |
    |Disk Capacity|The default recommended disk capacity varies with the traffic specification. If you adjust the traffic specification, the disk capacity is adjusted accordingly. You can adjust the disk capacity based on your business needs.|
    |Supported Topics|    -   Each time you purchase a topic, a quota of 16 partitions is added.
    -   The number of topics that you can create on a Professional Edition instance is twice the number of topics that you purchase. |

    **Note:**

    -   You can only upgrade but cannot downgrade instance specifications.
    -   When you upgrade instance specifications, brokers in the cluster restart one by one. This may bring the following risks:
        -   The client will temporarily disconnect and reconnect, which may cause a few errors.
        -   The messages that have been sent will not be lost after the upgrade. If a message fails to be sent during the upgrade, we recommend that you resend it. You can configure a retry mechanism on the client.
        -   The upgrade lasts for about 30 minutes. More time is consumed for a larger increase in disk capacity. The service is not interrupted but messages may be distributed to a different partition for consumption. Therefore, evaluate the impact on your business before you proceed. We recommend that you upgrade instance specifications during off-peak hours.
8.  Read and select the terms of service. Then, click **Buy Now**.

    **Note:** After you upgrade the specifications of your instance, the time when your order will take effect appears on the upgrade order page.


On the **Instance Details** page, the specifications after the upgrade appear.

