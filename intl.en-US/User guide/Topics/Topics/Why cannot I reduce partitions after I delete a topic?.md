# Why cannot I reduce partitions after I delete a topic?

After the routing information is completely cleared, you can specify a new number of partitions for the topic.

## Condition

If you delete a topic with n partitions and create this topic again by setting the partition count to a value smaller than n, the system displays a message, indicating that **the topic is created but the partition count cannot be smaller than the previously configured number, so the partition count is reset to n.**

## Cause

In earlier versions, the routing information cannot be completely cleared after a topic is deleted. As a result, you cannot create a topic with fewer partitions than those of the deleted topic. After you upgrade your instance to the latest version, the routing information of any topic you deleted in the source version is retained. To completely clear the routing information, you need to create a topic with the same configuration as the previously deleted topic and delete it again. Then, you can create a topic with any partition count as needed.

## Remedy

1.  Ensure that the internal version of the instance is the latest.

    In the Message Queue for Apache Kafka console, go to the **Instance Details** page. In the **Basic Information** section, view the current **Internal Version**.

    -   If **Latest Version** is displayed, you do not need to upgrade the version.
    -   If **Upgrade Minor Version** is displayed, click **Upgrade Minor Version** to upgrade the version.
2.  Create a topic with the same configuration as the previously deleted topic, delete it, and then create it again.

    Go to the **Topics** page, create a topic with the same configuration as the previously deleted topic, delete it, create it again, and set the partition count.


