# Upgrade the instance version

This topic describes how to upgrade the Message Queue for Apache Kafka instance version so that you can use the related features.

## Notes

-   The Message Queue for Apache Kafka broker of version 0.10 may trigger bugs such as deadlocks and frequent rebalancing. We recommend that you upgrade the instance from version 0.10 to the stable version 0.10.2.
-   If the **Open-Source Version** of the instance is 0.10 on the **Instance Details** page, and the upgrade button is available, you need to upgrade your instance to version 0.10.2.
-   All newly purchased Message Queue for Apache Kafka instances are of version 0.10.2. The Message Queue for Apache Kafka team will gradually schedule a mandatory upgrade for instances of version 0.10. We recommend that you upgrade them beforehand.

## Background

You can upgrade the open-source version or internal version of an instance to the required version in the Message Queue for Apache Kafka console. Upgrade of the two versions is compared as follows:

-   Open-source version upgrade \(major version upgrade\)

    Upgrade the open-source version of the running Message Queue for Apache Kafka instance. For example, upgrade the open-source version of the instance from version 0.10.2 to version 2.2.0.

    **Note:**

    -   The default deployment version of the Message Queue for Apache Kafka instance is version 0.10.x.
    -   Message Queue for Apache Kafka Standard Edition does not support upgrade of the instance version from version 0.10.x to version 2.x. You need to upgrade the Standard Edition to the Professional Edition. For more information, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade the instance configuration.md).
    -   Only the open-source versions 0.10.x and 2.x are supported for Message Queue for Apache Kafka instances.
    -   Version 2.x is compatible with version 0.11.x and version 1.x.
-   Internal version upgrade \(minor version upgrade\)

    Optimize the internal version of the running Message Queue for Apache Kafka instance. The open-source version of the instance does not change during the upgrade of the internal version. For example, the open-source version of an instance is version 0.10.2. After you upgrade the internal version of the instance, the open-source version of the instance is still version 0.10.2.


## Upgrade the open-source version of an instance

Prerequisites

-   You have purchased a Message Queue for Apache Kafka instance of the Professional Edition, and the instance is in the Running state.
-   The open-source version of your Message Queue for Apache Kafka instance is version 0.10.x.

Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance is located.
2.  In the left-side navigation pane, click **Instance Details**.
3.  On the Instance Details page, click the ID of the target instance.
4.  In the Basic Information section, when **Internal Version** is **Service Upgrade**, click **Service Upgrade** to upgrade the instance. When Internal Version is **Latest Version**, do not upgrade the instance.

    The instance version will be upgraded to the most suitable open-source version based on your specific instance conditions.

5.  In the Upgrade dialog box, click the **Cross-Version Upgrade** tab.
    1.  Enter your name in the **Name** field.
    2.  Enter your emergency phone number in the **Emergency Phone Number** field.
    3.  Select **Yes** for **Cross-Version Upgrade to 2.0**.
6.  Click **Upgrade**.

**Note:**

-   If the client does not support the reconnection mechanism \(enabled by default\), the client may be unavailable after being disconnected. Ensure that the consumer supports the reconnection mechanism.
-   It will take about 15 minutes for the upgrade. The service will not be interrupted during the upgrade and the business will not be affected.
-   The message storage format of instances of version 2.x is different from that of the instances of version 0.10.x. Therefore, you cannot roll back to version 0.10.x after the upgrade. Proceed with caution.
-   We recommend that you purchase a test instance for upgrade verification before you operate on the production instance.

Verification

The value of **Open-Source Edition** is **2.2.0** in the **Basic Information** section on the Instance Details page.

## Upgrade the internal version of an instance

Prerequisites

-   You have purchased a Message Queue for Apache Kafka instance, and the instance is in the Running state.
-   The internal version of your Message Queue for Apache Kafka instance is not the latest version.

Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com). In the top navigation bar, select the region where the instance is located.
2.  In the left-side navigation pane, click **Instance Details**.
3.  On the Instance Details page, click the ID of the target instance.
4.  In the Basic Information section, when **Internal Version** is **Service Upgrade**, click **Service Upgrade** to upgrade the instance. When Internal Version is **Latest Version**, do not upgrade the instance.

    The instance version will be upgraded to the most suitable internal version based on your specific instance conditions.

5.  In the Upgrade dialog box, set the following parameters so that we can contact you when an error occurs during the upgrade:
    -   Name
    -   Emergency phone number
6.  Click **Upgrade**.

    **Note:**

    -   If the client does not support the reconnection mechanism \(enabled by default\), the client may be unavailable after being disconnected. Ensure that the consumer supports the reconnection mechanism.
    -   The upgrade will take about 15 minutes. The service will not be interrupted during the upgrade and the business will not be affected.

Verification

The value of **Internal Version** is **Latest Version** in the **Basic Information** section on the Instance Details page.

