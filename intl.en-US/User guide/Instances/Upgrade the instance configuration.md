# Upgrade the instance configuration

If your Message Queue for Apache Kafka instance fails to meet your business requirements because the disk usage is continuously higher than 85% or the peak traffic continuously exceeds that you purchased, you can upgrade the instance configuration as needed.

## Background

-   Instance edition

    You can upgrade an instance from the Standard Edition to the Professional Edition.

-   Peak traffic

    The peak traffic is restricted as follows during the upgrade:

    -   If the purchased peak traffic is less than 120 MB/s, you can upgrade it to 120 MB/s at most.
    -   If the purchased peak traffic is greater than 120 MB/s and no more than 300 MB/s, you can upgrade it to 300 MB/s at most.
    -   If the purchased peak traffic is greater than 300 MB/s, you cannot upgrade it.
-   Disk type

    The disk types are restricted as follows during the upgrade:

    -   The disk type cannot be changed after the order is placed. Select the disk type with caution.
    -   Ultra disks support peak traffic upgrade to 120 MB/s at most.
    -   Solid State Drives \(SSDs\) support peak traffic upgrade to 300 MB/s at most.

## Notes

Upgrading the instance specifications will cause instances in the cluster to restart one by one.

-   If the client does not support the reconnection mechanism, the client may be unavailable after being disconnected.
-   It will take about 30 minutes to upgrade the instance specifications. Services will not be interrupted but the messages may be out of order during the upgrade, that is, messages may be distributed to a different partition for consumption. Therefore, evaluate the impact on businesses before you proceed.

## Prerequisites

This topic describes how to upgrade a subscription Message Queue for Apache Kafka instance from the Standard Edition to the Professional Edition.

-   The billing method is subscription.
-   The instance is of the Standard Edition and it is in the **Running** state.

## Procedure

To upgrade a subscription Message Queue for Apache Kafka instance from the Standard Edition to the Professional Edition, follow these steps:

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com) and select a region in the top navigation bar.
2.  In the left-side navigation pane, click **Instance Details**.
3.  On the top of the Instance Details page, click the ID of the target instance. Click **Upgrade Instance** in the upper-right corner of the **Basic Information** section.

    ![upgrade_instance](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p53350.png)

    **Note:** You can click the edit icon next to **Instance Name** and modify the instance as needed.

4.  In the Instance Upgrade Risks dialog box, read the instance upgrade risks carefully. Confirm the risks, select **I am aware of the service unavailability risk caused by the restart of servers in the cluster.** and then click **I am aware of the risks.**

    ![instance_upgrade_risks](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p53351.png)

5.  On the Configuration Change page, change configuraiton, select **Message Queue for Apache Kafka \(Subscription\) Terms of Service**, and then click **Pay**.

    ![配置变更 ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7840549951/p93965.png)

    **Note:**

    -   The recommended disk capacity is available for peak traffic. The disk capacity changes with the adjusted peak traffic.
    -   A longer time is consumed for a larger disk capacity span.
    -   After the specifications are upgraded, the effective time of the order is subject to that displayed on the upgrade order page.

## Verification

Check the **Status** section on the Instance Details page.

-   If the status is **Running**, the modification is successful.
-   If the status is **Upgrading**, wait for a while.
-   If the status is **Upgrade Failed**, submit a [ticket](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.dticket.6aab12d2AGzO5u#/ticket/createIndex).

## What to do next

After the instance specifications are upgraded, you may need to modify the message configuration to adapt to the target instance edition. For more information, see [Modify the message configuration](/intl.en-US/User guide/Instances/Modify the message configuration.md).

## References

-   For more information about the disk usage or peak traffic, see [Monitor resources and set alerts](/intl.en-US/User guide/Alerts/Monitor resources and set alerts.md).
-   For more information about billing methods and instance editions, see [Billing](/intl.en-US/Pricing/Billing.md).

