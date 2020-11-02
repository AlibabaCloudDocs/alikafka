# Why does a deleted consumer group still receive messages?

When a consumer group for an instance of an earlier version is deleted, its routing information is not completely cleared.

## Symptom

A deleted consumer group still receives messages.

## Cause

In earlier versions, when a consumer group is deleted, its routing information is not completely cleared. Therefore, a deleted consumer group can still receive messages. After you upgrade your instance to the latest version, the routing information of any consumer group you deleted in the source version is retained. To completely clear the routing information, you need to create a consumer group with the same configuration as the previously deleted consumer group and then delete it. Then, you can create a consumer group with the same configuration again.

## Solution

1.  Upgrade the instance version to the latest version.

    Upgrade the instance version to the latest version.

    In the Message Queue for Apache Kafka console, go to the **Instance Details** page. In the **Basic Information** section, view the current **Internal Version**.

    -   If **Latest Version** is displayed, you do not need to upgrade the version.
    -   If **Upgrade Minor Version** is displayed, click **Upgrade Minor Version** to upgrade the version.
2.  Create a consumer group with the same configuration as the previously deleted consumer group, delete it, and then create it again.

    Go to the **Consumer Groups** page, create a consumer group with the same configuration as the previously deleted consumer group, delete it, and then create it again.


