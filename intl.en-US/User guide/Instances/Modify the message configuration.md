---
keyword: [kafka, modify, configurations]
---

# Modify the message configuration

You can adjust the message retention period and the maximum message size based on business needs.

A Message Queue for Apache Kafka instance is purchased and deployed, and it is in the Running state.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](http://kafka.console.aliyun.com).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instance Details** page, click the instance whose message configuration you want to modify.

5.  In the **Configurations** section, click **Configuration Change**.

6.  In the **Configuration Change** dialog box, set the parameters and then click **Change**.

    ![Modify the message configuration](../images/p120810.png)

    |Parameter|Description|
    |---------|-----------|
    |Message Retention Period|The maximum message retention period when the disk capacity is sufficient.     -   When the disk usage reaches 85%, the disk capacity is insufficient, and the system deletes messages from the earliest stored ones to ensure service availability.
    -   The default value is 72 hours. Valid values: 24 hours to 480 hours. |
    |Maximum Message Size|The maximum size of a message that Message Queue for Apache Kafka can receive and send.     -   The maximum message size is 10 MB for instances of the Standard Edition and Professional Edition.
    -   Before you modify the configuration, ensure that the new value matches the configuration on the producer and consumer. |

7.  In the **Alerts** dialog box, select **I am aware of the service unavailability risk caused by restart of servers in the cluster.** and then click **I am aware of the risks.**.

    ![I am aware of the risks](../images/p120817.png)


[View task execution records](/intl.en-US/User guide/Instances/View the execution records of tasks.md)

