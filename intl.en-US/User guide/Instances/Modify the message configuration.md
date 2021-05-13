---
keyword: [kafka, change, configuration]
---

# Modify the message configuration

You can adjust the message retention period and the maximum message size based on business needs.

A Message Queue for Apache Kafka instance is purchased and deployed, and it is in the Running state.

## Procedure

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select the region where your instance is located.

3.  In the left-side navigation pane, click **Instances**.

4.  On the **Instances** page, click the name of the instance that you want to manage.

5.  In the **Configurations** section of the **Instance Details** page, click **Configuration Change**.

6.  In the **Configuration Change** dialog box, set the parameters and click **Change**.

    ![Modify the message configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0958844061/p120810.png)

    |Parameter|Description|
    |---------|-----------|
    |Message Retention Period|The maximum message retention period when the disk capacity is sufficient.     -   When the disk usage reaches 85%, the disk capacity is insufficient. In this case, the system deletes messages from the earliest stored ones to ensure service availability.
    -   Valid values: 24 hours to 480 hours. Default value: 72 hours. |
    |Maximum Message Size|The maximum size of a message that you can send and receive in Message Queue for Apache Kafka.     -   The maximum message size is 10 MB for instances of both the Standard Edition and the Professional Edition.
    -   Before you modify the configuration, make sure that the new value matches the configuration on the producer and consumer. |

7.  In the **Alerts** dialog box, select **I am aware of the service unavailability risk caused by restart of servers in the cluster.** and click **I am aware of the risks.**.

    ![I am aware of the risks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0958844061/p120817.png)


[View task execution records](/intl.en-US/User guide/Instances/View task execution records.md)

