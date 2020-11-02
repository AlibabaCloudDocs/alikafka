---
keyword: [kafka, specification, upgrade]
---

# Upgrade instance specifications

You can upgrade the specifications of your Message Queue for Apache Kafka instances. For example, you may want to upgrade your Message Queue for Apache Kafka instance if the disk usage is continuously higher than 85% or peak traffic continuously exceeds the amount that you purchased.

A Message Queue for Apache Kafka instance is purchased and deployed. For more information, see [Access from a VPC](/intl.en-US/Quick-start/Step 2: Purchase and deploy an instance/Connect Message Queue for Apache Kafka to a VPC.md).

Message Queue for Apache Kafka allows you to upgrade an instance in the following dimensions:

|Item|Description|
|----|-----------|
|Edition|You can upgrade an instance from the Standard Edition to the Professional Edition.|
|Traffic specification|The following limits apply to upgrading the traffic specification of an instance:-   Edition
    -   Standard Edition: supports peak traffic of up to 120 MB/s. If your peak traffic exceeds 120 MB/s, upgrade your instance to the Professional Edition and then upgrade the traffic specification.
    -   Professional Edition: supports peak traffic of up to 2,000 MB/s.
-   Disk type
    -   Ultra disk: If peak traffic exceeds 120 MB/s, the corresponding cluster will be scaled out. After the upgrade is complete, you must rebalance topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md).
    -   SSD: If peak traffic exceeds 300 MB/s, the corresponding cluster will be scaled out. After the upgrade is complete, you must rebalance topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md). |
|Topic specification|-   Each additional topic increases the number of partitions by 16.
-   The number of usable topics for a Professional Edition instance is twice the number purchased. |

## Precautions

-   You can only upgrade but cannot downgrade instance specifications.
-   When you upgrade instance specifications, brokers in the cluster restart one by one. This may incur the following risks:
    -   Errors may occur because the client temporarily disconnects from the broker and then reconnects.
    -   Sent messages are not discarded after an upgrade. If a message fails to be sent during the upgrade, resend the message after the upgrade is complete. You can configure the retry mechanism on the client.
    -   Upgrading an instance takes about 30 minutes and does not interrupt the service. However, messages may be distributed to a different partition for consumption during the upgrade. Therefore, evaluate the potential impact on your business before you upgrade an instance.
-   We recommend that you upgrade an instance during off-peak hours.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select your region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instance Details** page, select the target instance. In the upper-right corner of the **Basic Information** section, click **Upgrade Instance**.

5.  In the **Instance Upgrade Risks** dialog box, read the risks of upgrading an instance and select **I am aware of the service unavailability risk caused by restart of servers in the cluster.** Then click **I am aware of the risks.**

6.  On the **Upgrade** page, set parameters and read the terms of service. Confirm that you have read the terms of service, and then click **Buy Now**.

    **Note:**

    -   Each traffic specification has a recommended disk capacity. If you change the traffic specification, the default value for disk capacity will change.
    -   Upgrading an instance takes longer for larger increases in disk capacity.
    -   After you upgrade instance specifications, the time when your order will take effect is shown on the upgrade order page.

The post-upgrade specification is shown on the **Instance Details** page.

