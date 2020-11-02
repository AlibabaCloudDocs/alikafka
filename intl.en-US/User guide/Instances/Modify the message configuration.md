# Modify the message configuration

You can adjust the message retention period and the maximum message size based on the business requirements.

## Background

Modifying the message configuration will cause instances in the cluster to restart one by one.

-   If the client does not support the reconnection mechanism, the client may be unavailable after being disconnected.
-   It will take 15 minutes to 30 minutes to modify the message configuration. Services will not be interrupted but the messages may be out of order during the upgrade, that is, messages may be distributed to a different partition for consumption. Therefore, evaluate the impact on businesses before you proceed.

## Prerequisites

You have purchased a Message Queue for Apache Kafka instance, and the instance is in the Running state.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com). In the top navigation bar, select a region where the instance is located.
2.  In the left-side navigation pane, click **Instance Details**.
3.  On the Instance Details page, click the target instance.
4.  In the **Configurations** section, click **Configuration Change**.
5.  In the Configuration Change dialog box, set parameters and click **Change**.

    |Parameter|Description|
    |---------|-----------|
    |Message Retention Period|The maximum message retention period when the disk capacity is sufficient.     -   When the disk capacity is insufficient \(that is, when the disk usage reaches 85%\), the old messages are deleted in advance to ensure service availability.
    -   The default value is 72 hours. The value ranges from 24 hours to 168 hours. |
    |Maximum Message Length|The maximum size of a message that can be sent and received by Message Queue for Apache Kafka.     -   The upper limit for maximum message length is 10 MB, with no difference between the standard edition instance and the professional edition instance.
    -   Before modifying the configuration, ensure that the target value matches the configuration on the producer and consumer. |

6.  In the Alerts dialog box, select **I am aware of the service unavailability risk caused by restart of servers in the cluster.** and then click **I am aware of the risks.**

## Verification

Check the **Status** section on the Instance Details page.

-   If the status is **Running**, the modification is successful.
-   If the status is **Upgrading**, wait for a while.

