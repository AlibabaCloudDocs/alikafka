---
keyword: [kafka, rebalance the topic traffic, scale out]
---

# Rebalance the topic traffic

When you upgrade the traffic specification of a Message Queue for Apache Kafka instance, the corresponding cluster may be scaled out. After the cluster is scaled out, you must rebalance the topic traffic to distribute the traffic evenly across brokers in the scaled-out cluster. Otherwise, the original topic traffic is still distributed to the original brokers before the scale-out. The peak traffic of the original topics is limited by the traffic specification before the scale-out. The traffic of the added topics is not subject to the traffic specification before the scale-out.

Your Message Queue for Apache Kafka instance is in the **Running \(Pending Rebalancing\)** state.

**Note:** For more information about how to upgrade the traffic specification of an instance and when cluster scale-out is triggered, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).

## Precautions

When your Message Queue for Apache Kafka instance is in the **Running \(Pending Rebalancing\)** state, you can use this instance to send and subscribe to messages but cannot create resources such as topics and consumer groups in this instance. You must complete topic traffic rebalancing or choose not to rebalance topic traffic before you can create a resource.

## Traffic rebalancing method

The following table describes the traffic rebalancing methods supported by Message Queue for Apache Kafka.

|Traffic rebalancing method|Principle|Impact|Scenario|Duration|
|--------------------------|---------|------|--------|--------|
|Add Partitions to All Topics|Add partitions to the new brokers after the cluster scale-out for all topics on the original brokers.|-   New messages in partitions are out of order.
-   The number of partitions changes. If your client cannot automatically detect new partitions in scenarios such as stream computing or delivery of messages to and consumption of messages from specified partitions, you may need to restart the client or modify the client code.

|-   The partition order is not required.
-   The partition to which messages are sent is not specified.
-   The consumption method is Subscribe.

|Seconds.|
|Migrate Partitions of All Topics \(recommended\)|-   Local storage: The kafka-reassign-partitions tool is used to migrate topic data in partitions.
-   Cloud storage: The mapping is modified and the topic data in partitions is not migrated.

|-   Local storage: Temporary internal traffic is generated.
-   Cloud storage: No temporary internal traffic is generated.

|All cluster scale-out scenarios are supported.|-   Local storage: minutes or hours. This duration depends on the amount of the data you want to migrate from local storage. If the data volume is large, data migration may take several hours or longer. Therefore, you must evaluate the impact. We recommend that you migrate data during off-peak hours of service traffic.
-   Cloud storage: seconds. The migration of the data of one topic takes about 30 seconds. |
|Do Not Rebalance \(not recommended\)|You do not need to perform any operations. The original topics are still distributed on the brokers of the cluster before the scale-out, and the new topics are evenly distributed across all brokers after the scale-out.|-   The peak traffic of the original topics are subject to the traffic specification before the scale-out.
-   If the original topic traffic is large, the traffic between brokers may be unbalanced.

|-   The original topic traffic is very small, and the original topic traffic is not greatly improved after the cluster is scaled out.
-   New topics are created after the cluster is scaled out. Most of the traffic is distributed to the new topics.

|Takes effect immediately.|

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instance Details** page, click an instance. In the **Status** section, click **Rebalance Now**.

5.  In the **Rebalancing Method** dialog box, select a rebalancing method. The following rebalancing methods are supported:

    -   Add Partitions to All Topics

        Select **Add Partitions to All Topics** and click **OK**.

    -   Migrate Partitions of All Topics
        1.  Submita [ticket](https://workorder-intl.console.aliyun.com/?spm=a2c63.p38356.879954.5.7eda4058RBvAh8#/ticket/add/?productId=1352) and ask Message Queue for Apache Kafka Customer Services to upgrade your broker to the latest version.
        2.  Select **Migrate Partitions of All Topics** and click **OK**.
    -   Do Not Rebalance

        Select **Do Not Rebalance** and click **OK**.


After topic traffic is rebalanced, the instance status is **Running**.

