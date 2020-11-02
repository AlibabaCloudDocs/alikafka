# Upgrade the instance version

This topic describes how to upgrade the Message Queue for Apache Kafka instance version.

The Message Queue for Apache Kafka instance is in the **Running** state.

-   Upgrade the major version of an instance

    Major version upgrade refers to a cross-version upgrade. For example, you can upgrade a Message Queue for Apache Kafka instance from version 0.10.x to version 2.x.

    **Note:**

    -   By default, major version 0.10.x of the Message Queue for Apache Kafka instance is deployed. The default version is 0.10.2 for a new instance and 0.10 for an old instance. Version 0.10 may trigger bugs such as deadlocks and frequent rebalancing. We recommend that you upgrade the instance from version 0.10 to version 0.10.2. For more information about the upgrade, see [Upgrade the minor version of an instance](#section_3te_xlw_iip).
    -   Message Queue for Apache Kafka instances support major versions 0.10.x and 2.x. Major version 0.10.x provides 0.10 and 0.10.2, while major version 2.x only provides 2.2.0.
-   Upgrade the minor version of an instance

    Minor version upgrade refers to a non-cross-version upgrade. For example, you can upgrade a Message Queue for Apache Kafka instance from 0.10 to 0.10.2, or from version 0.10.2 to the 0.10.2 kernel-optimized version.


## Upgrade the major version of an instance

The major version of a Message Queue for Apache Kafka Standard Edition instance cannot be upgraded from 0.10.x to 2.x. You need to first upgrade the instance from the Standard Edition to the Professional Edition and then upgrade the major version. For more information about how to upgrade the instance edition, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).

**Note:**

-   The upgrade is free of charge and compatible with the existing SDK and API.
-   To avoid impact on your business during the upgrade, ensure that your client supports automatic reconnection and can handle disconnections. By default, the client supports automatic reconnection.
-   It will take about 25 minutes for the upgrade. The service will not be interrupted during the upgrade and the business will not be affected.
-   Instances of version 2.x use a different message storage format from that of instances of version 0.10.x. Therefore, you cannot roll back to version 0.10.x after the upgrade. Proceed with caution.
-   We recommend that you purchase a test instance for upgrade verification before you upgrade the production instance.
-   We recommend that you perform the upgrade during off-peak hours.
-   We also recommend that you update the client version after the upgrade to keep the same versions of the client and broker.

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instance Details**.

4.  On the **Instance Details** page, click an instance. In the **Basic Information** section, click **Upgrade Major Version** next to **Open-Source Version**.

5.  In the **Upgrade Major Version** dialog box, perform the following steps:

    1.  Enter your name in the **Name** field.

    2.  Enter your emergency phone number in the **Emergency phone number:** field.

    3.  Select **Yes** for **Cross-Version Upgrade to 2.0**.

    4.  Click **Upgrade**.


## Upgrade the minor version of an instance

**Note:**

-   The upgrade is free of charge and compatible with the existing SDK and API.
-   To avoid impact on your business during the upgrade, ensure that your client supports automatic reconnection and can handle disconnections. By default, the client supports automatic reconnection.
-   It will take about 15 minutes for the upgrade. Services will not be interrupted during the upgrade and the business will not be affected.
-   We recommend that you perform the upgrade during off-peak hours.
-   We also recommend that you update the client version after the upgrade to keep the same versions of the client and broker.

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instance Details**.

4.  On the **Instance Details** page, click an instance In the **Basic Information** section, click **Upgrade Minor Version** next to **Internal Version**.

5.  In the **Upgrade Minor Version** dialog box, perform the following steps:

    1.  Enter your name in the **Name** field.

    2.  Enter your emergency phone number in the **Emergency phone number:** field.

    3.  Click **Upgrade**.


