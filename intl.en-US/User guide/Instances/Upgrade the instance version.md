# Upgrade the instance version

This topic describes how to upgrade the version of a Message Queue for Apache Kafka instance.

The Message Queue for Apache Kafka instance is in the **Running** state.

-   Upgrade the major version of an instance

    A major version upgrade is an upgrade from one major version to another major version. For example, you can upgrade a Message Queue for Apache Kafka instance from version 0.10.x to version 2.x.

    **Note:**

    -   By default, the major version deployed for a Message Queue for Apache Kafka instance is 0.10.x. The default version is 0.10.2 for a new instance and 0.10 for an old instance. Version 0.10 may trigger bugs such as deadlocks and frequent rebalancing. We recommend that you upgrade the instance from version 0.10 to version 0.10.2. For more information about the upgrade, see [Minor version upgrade](#section_3te_xlw_iip).
    -   Message Queue for Apache Kafka instances support major versions 0.10.x and 2.x. Major version 0.10.x provides 0.10 and 0.10.2, while major version 2.x provides only 2.2.0.
-   Upgrade the minor version of an instance

    A minor version upgrade is an upgrade from one minor version to another minor version. For example, you can upgrade a Message Queue for Apache Kafka instance from version 0.10 to version 0.10.2, or from version 0.10.2 to the 0.10.2 kernel-optimized version.


## Major version upgrade

The major version of a Message Queue for Apache Kafka Standard Edition instance cannot be upgraded from 0.10.x to 2.x. To upgrade the major version, you must first upgrade the instance from the Standard Edition to the Professional Edition. For more information about how to upgrade the instance edition, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).

**Note:**

-   The upgrade is free of charge and compatible with the existing SDK and API.
-   To avoid impact on your business during the upgrade, make sure that your client supports automatic reconnection and can handle disconnections. By default, the client supports automatic reconnection.
-   The upgrade takes about 25 minutes. During the upgrade, the service will not be interrupted, and your business will not be affected in normal cases.
-   Instances of version 2.x use a different message storage format from that of instances of version 0.10.x. Therefore, you cannot roll back to version 0.10.x after the upgrade. Proceed with caution.
-   We recommend that you purchase a test instance for upgrade verification before you upgrade your production instance.
-   We recommend that you perform the upgrade during off-peak hours.
-   We also recommend that you update the client version after the upgrade to keep the versions of the client and broker consistent.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the **Basic Information** section of the **Instance Details** tab on the **Instance Details** page, click **Upgrade Major Version** on the right of **Major Version:**.

6.  In the **Upgrade Major Version** dialog box, perform the following operations:

    1.  Enter your name in the **Name:** field.

    2.  Enter your emergency phone number in the **Emergency phone number:** field.

    3.  Select **Yes** for **Cross-Version Upgrade to 2.0**.

    4.  Click **Upgrade**.


## Minor version upgrade

**Note:**

-   The upgrade is free of charge and compatible with the existing SDK and API.
-   To avoid impact on your business during the upgrade, make sure that your client supports automatic reconnection and can handle disconnections. By default, the client supports automatic reconnection.
-   The upgrade takes about 15 minutes. During the upgrade, the service will not be interrupted, and your business will not be affected in normal cases.
-   We recommend that you perform the upgrade during off-peak hours.
-   We also recommend that you update the client version after the upgrade to keep the versions of the client and broker consistent.

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the **Basic Information** section of the **Instance Details** tab on the **Instance Details** page, click **Upgrade Minor Version** on the right of **Minor Version:**.

6.  In the **Upgrade Minor Version** dialog box, perform the following operations:

    1.  Enter your name in the **Name:** field.

    2.  Enter your emergency phone number in the **Emergency phone number:** field.

    3.  Click **Upgrade**.


