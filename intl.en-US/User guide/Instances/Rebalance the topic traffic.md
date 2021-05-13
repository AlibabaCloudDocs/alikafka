---
keyword: [kafka, rebalance topic traffic, scale out]
---

# Rebalance the topic traffic

When you upgrade the traffic specification of a Message Queue for Apache Kafka instance, the corresponding cluster may be scaled out. After the cluster is scaled out, you must rebalance the topic traffic to distribute the traffic evenly across brokers in the scaled-out cluster. Otherwise, the original topic traffic is still distributed across the brokers that are in the cluster before the scale-out. The original topics are subject to the maximum traffic purchased before the scale-out. The new topics are not subject to this maximum traffic.

Your Message Queue for Apache Kafka instance is in the **Running \(Pending Rebalancing\)** state.

**Note:** For more information about how to upgrade the traffic specification of an instance and when cluster scale-out is triggered, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).

## Usage notes

When your Message Queue for Apache Kafka instance is in the **Running \(Pending Rebalancing\)** state, you can use this instance to send and receive messages but cannot create resources such as topics and consumer groups in this instance. You must complete topic traffic rebalancing or choose not to rebalance topic traffic before you can create a resource.

## Traffic rebalancing methods

The following table describes the traffic rebalancing methods supported by Message Queue for Apache Kafka.

|Traffic rebalancing method|Principle|Impact|Scenario|Duration|
|--------------------------|---------|------|--------|--------|
|Add Partitions to All Topics|After the cluster is scaled out, the system adds partitions to the new brokers for all topics on the original brokers.|-   New messages in partitions are out of order.
-   The number of partitions changes. If your client cannot automatically detect new partitions, you may need to restart the client or modify the client code. This may occur in scenarios where stream computing is performed or you send messages to or consume messages from specified partitions.

|-   The partition order is not required.
-   The partition to which messages are sent is not specified.
-   The consumption method is Subscribe.

|Seconds.|
|Migrate Partitions of All Topics \(recommended\)|-   Local storage: The kafka-reassign-partitions tool is used to migrate topic data in partitions.
-   Cloud storage: The mapping is modified and the topic data in partitions is not migrated.

|-   Local storage: Temporary internal traffic is generated.
-   Cloud storage: No temporary internal traffic is generated.

|All cluster scale-out scenarios are supported.|-   Local storage: minutes or hours. This duration depends on the amount of the data that you want to migrate from local storage. If the data volume is large, the migration may take several hours or longer. We recommend that you migrate the data during off-peak hours.
-   Cloud storage: seconds. It takes about 30 seconds to migrate a topic. |
|Do Not Rebalance \(not recommended\)|You do not need to perform operations. The original topics are still distributed on the brokers of the cluster before the scale-out, and the new topics are evenly distributed across all brokers after the scale-out.|-   The original topics are subject to the maximum traffic purchased before the scale-out.
-   If the original topic traffic is large, the traffic between brokers may be unbalanced.

|-   The original topic traffic is small, and is not greatly improved after the cluster is scaled out.
-   New topics are created after the cluster is scaled out. Most of the traffic is directed to the new topics.

|Immediately takes effect.|

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the **Status** section of the **Instance Details** page, click **Rebalance Now**.

6.  In the **Rebalancing Method** dialog box, select a rebalancing method. The following rebalancing methods are supported:

    -   Add Partitions to All Topics

        Select **Add Partitions to All Topics** and click **OK**.

    -   Migrate Partitions of All Topics
        1.  Submit a [ticket](https://workorder-intl.console.aliyun.com/?spm=a2c63.p38356.879954.5.7eda4058RBvAh8#/ticket/add/?productId=1352) and ask Message Queue for Apache Kafka customer service to upgrade your broker to the latest version.
        2.  Select **Migrate Partitions of All Topics** and click **OK**.
    -   Do Not Rebalance

        Select **Do Not Rebalance** and click **OK**.


After topic traffic is rebalanced, the instance status is **Running**.

