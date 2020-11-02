# What can I do if Insufficient Data is displayed for Status in the Alert Items dialog box?

## Problem description

Log on to the Message Queue for Apache Kafka console and go to the Monitoring and Alerts page. In the **Alert Items** column, click **Alert Items: x**. In the Alert Items dialog box, the **Status** column displays **Insufficient Data**.

## Causes

The Message Queue for Apache Kafka instance is not upgraded to the version that supports alert data reporting after alert rules are configured.

## Solutions

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance is located.

2.  In the left-side navigation pane, click **Instances**.

3.  On the Instance Details page, click the ID of the target instance.

4.  In the Basic Information section, when **Internal Version** is **Service Upgrade**, click **Service Upgrade** to upgrade the instance. When Internal Version is **Latest Version**, do not upgrade the instance.

    The instance version will be upgraded to the most suitable internal version based on your specific instance conditions.

5.  In the Upgrade dialog box, set the following parameters so that we can contact you when an error occurs during the upgrade:

    -   Name
    -   Emergency Phone Number
6.  Click **Upgrade**.

    **Note:**

    -   If the client does not support the reconnection mechanism \(enabled by default\), the client may be unavailable after being disconnected. Ensure that the consumer supports the reconnection mechanism.

    -   The upgrade will take about 15 minutes. The service will not be interrupted during the upgrade and the business will not be affected.


