# Upgrade instance specifications

If your Message Queue for Apache Kafka instance fails to meet your business requirements because the disk usage is continuously higher than 85% or the peak traffic continuously exceeds your purchased peak traffic, you can upgrade your instance specifications as needed.

## Notes

-   Currently, you can only upgrade instance specifications.
-   Upgrading instance specifications causes brokers in the corresponding cluster to restart one by one. This creates the following risks:
    -   If a client is not configured with a reconnection mechanism, the Message Queue for Apache Kafka service may become unavailable after this client is disconnected from a corresponding broker.
    -   The upgrade takes about 30 minutes and does not interrupt the service. However, messages may be distributed to a different partition for consumption during the upgrade. Therefore, evaluate the impact on your business before you proceed.

## Instance specifications

Message Queue for Apache Kafka allows you to upgrade an instance in the following dimensions:

-   Instance edition

    You can upgrade an instance from the Standard Edition to the Professional Edition.

-   Peak traffic

    When you upgrade the peak traffic of an instance, take note of the following:

    -   Instance edition
        -   Standard Edition: supports the peak traffic of up to 120 MB/s. If you need the peak traffic higher than 120 MB/s, you must first upgrade your instance to the Professional Edition and then upgrade the peak traffic. For more information, see [Procedure](#section_qxf_mp2_kpn).
        -   Professional Edition: supports the peak traffic of up to 2,000 MB/s.
    -   Disk type
        -   Ultra disk: If the peak traffic exceeds 120 MB/s, the corresponding cluster is scaled out. After the upgrade is complete, you need to rebalance the topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md).
        -   SSD: If the peak traffic exceeds 300 MB/s, the corresponding cluster is scaled out. After the upgrade is complete, you need to rebalance the topic traffic. For more information, see [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md).
-   Disk capacity

    You cannot change the disk type of an instance.

-   Topic specifications
    -   For each additional topic purchased, the number of partitions increases by 16.
    -   For instances of the Professional Edition, the actual number of available topics is twice the number of the purchased topics.

## Prerequisites

This topic describes how to upgrade a subscription Message Queue for Apache Kafka instance from the Standard Edition to the Professional Edition.

-   The billing method is subscription.
-   The instance is of the Standard Edition and is in the **Running** state.

## Procedure

To upgrade a Message Queue for Apache Kafka instance from the Standard Edition to the Professional Edition, follow these steps:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance is located.
2.  In the left-side navigation pane, click **Instances**.
3.  On the top of the **Instance Details** page, click the ID of the target instance. In the **Basic Information** section, click **Upgrade Instance** in the upper-right corner.

    ![instance_detail](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p53350.png)

    **Note:** You can click the Edit icon next to the instance name to modify the name of your instance.

4.  In the **Instance Upgrade Risks** dialog box, read the risks of instance upgrade carefully. After you understand these risks, select **I am aware of the service unavailability risk caused by restart of servers in the cluster.**, and then click **I am aware of the risks.**.

    ![upgrade_risk](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p53351.png)

5.  On the **Configuration Change** page, modify the instance specifications, and select **Terms of Service of Message Queue for Apache Kafka \(Subscription\)**. Then click **Buy Now**.

    ![Change instance specifications](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p93965.png)

    **Note:**

    -   The recommended disk capacity is available for the corresponding peak traffic. The disk capacity changes with the adjusted peak traffic.
    -   A longer time is consumed for a larger increase in disk capacity.
    -   After the instance specifications are upgraded, the time for the order to take effect is subject to the time displayed on the upgrade order page.

## Verify the result

On the **Instance Details** page, check the status of the instance in the **Status** section.

-   If the status is **Running**, the upgrade is successful.
-   If the status is **Upgrading**, please wait.
-   If the status is **Upgrade Failed**, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.dticket.3a774bd36auiRh#/ticket/createIndex).

## What to do next

After the instance specifications are upgraded, you may need to modify the message configuration to adapt to the upgraded instance specifications. For more information, see [Modify the message configuration](/intl.en-US/User guide/Instances/Modify the message configuration.md).

## More information

-   For more information about the disk usage or peak traffic, see [Monitor resources and set alerts](/intl.en-US/User guide/Alerts/Monitor resources and set alerts.md).
-   For more information about billing methods and instance editions, see [Billing](/intl.en-US/Pricing/Billing.md).

