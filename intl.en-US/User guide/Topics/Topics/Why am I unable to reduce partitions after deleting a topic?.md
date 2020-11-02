# Why am I unable to reduce partitions after deleting a topic?

## Problem description

After you delete a topic with XX partitions and create this topic again with the partition count set to a value less than XX, the system prompts that **the topic is created but the partition count cannot be less than the previously configured number, so the partition count is reset to XX.**

## Causes

In earlier versions, after a topic is deleted, its routing information is not completely cleared. As a result, you cannot create a topic with fewer partitions than those of the deleted topic. After you upgrade your instance to the new version, the routing information about any topic you deleted in the original version is still retained. To completely clear the routing information, you need to create the previously deleted topic and then delete it. Then, you can create a topic with any partition count as needed.

## Solutions

1.  Upgrade the instance version to the latest version.

    In the Message Queue for Apache Kafka console, go to the **Instance Details** page. In the **Basic Information** section, view the current **Internal Version**.

    -   If **Latest Version** is displayed, you do not need to upgrade the version.
    -   If **Service Upgrade** is displayed, click **Service Upgrade** to upgrade the version.
2.  Create -\> Delete -\> Create again

    Go to the **Topics** page, create the previously deleted topic, delete it, and then create a topic and set the partition count.


