---
keyword: [Kafka, Migration]
---

# Overview

This topic describes the advantages, principle, and process of migrating a user-created Kafka cluster to a Message Queue for Apache Kafka instance.

## Advantages

For more information about the advantages of migrating a user-created Kafka cluster to a Message Queue for Apache Kafka instance, see [Benefits](/intl.en-US/Introduction/Benefits.md).

## How it works

To migrate a cluster for message queues, you only need to consume all messages in the old cluster. The producers and consumers are deployed in clusters and can be operated one by one, without being perceived by upper-layer services.

## Procedure

Perform the following operations to migrate a user-created Kafka cluster to a Message Queue for Apache Kafka instance:

1.  Evaluate the specifications of the user-created Kafka cluster to determine the edition of the Message Queue for Apache Kafka instance you want to purchase.

    For more information, see [Evaluate specifications](/intl.en-US/User guide/Migration/Evaluate specifications.md).

2.  Purchase a Message Queue for Apache Kafka instance and deploy it based on the recommendation.

    ![dg_migrate_2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5350549951/p137634.png)

3.  Migrate the topics of the user-created Kafka cluster to the Message Queue for Apache Kafka instance.

    For more information, see [Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka](/intl.en-US/User guide/Migration/Migrate topics/Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka.md).

    ![dg_migrate_3](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5350549951/p137635.png)

4.  Migrate the consumer groups of the user-created Kafka cluster to the Message Queue for Apache Kafka instance.

    For more information, see [Migrate consumer group metadata from a user-created Kafka cluster to Message Queue for Apache Kafka](/intl.en-US/User guide/Migration/Migrate consumer groups/Migrate consumer group metadata from a user-created Kafka cluster to Message Queue for Apache Kafka.md).

    ![dg_migrate_4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5350549951/p137636.png)

5.  Migrate the data of the user-created Kafka cluster to the Message Queue for Apache Kafka instance.

    **Note:** For message queues, after the data of a cluster is consumed, the data will not be used except for backup. Therefore, generally, we recommend that you do not migrate data except that you must back up data of the user-created Kafka cluster to the Message Queue for Apache Kafka instance.

    For more information, see [Migrate data to the cloud](/intl.en-US/User guide/Migration/Migrate data/Migrate data to the cloud.md).

    ![dg_migrate_5](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5350549951/p137637.png)

6.  Enable a new consumer group for the Message Queue for Apache Kafka instance to consume messages of the Message Queue for Apache Kafka instance.

    ![dg_migrate_6](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5350549951/p137638.png)

7.  Enable a new producer for the Message Queue for Apache Kafka instance, deprecate the old producer, and enable the old consumer group to consume messages of the user-created Kafka cluster.

    ![dg_migrate_7](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6350549951/p137639.png)

8.  After all messages of the user-created Kafka cluster are consumed by the old consumer group, deprecate the old consumer group and the user-created Kafka cluster.

    ![dg_migrate_8](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6350549951/p137642.png)


