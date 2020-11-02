---
keyword: [kafka, cloud migration, specification]
---

# View the migration progress

This topic describes how to view the progress of migrating data from a user-created Kafka cluster to Message Queue for Apache Kafka.

The following operations are completed:

1.  Purchase and deploy a Message Queue for Apache Kafka instance.
2.  Start to migrate data from a user-created Kafka cluster to a Message Queue for Apache Kafka instance. The following migration types are supported:
    -   Metadata migration
        -   [Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache Kafka](/intl.en-US/User guide/Migration/Migrate topics/Migrate topic metadata from a user-created Kafka cluster to Message Queue for Apache
         Kafka.md)
        -   [Migrate consumer group metadata from a user-created Kafka cluster to Message Queue for Apache Kafka](/intl.en-US/User guide/Migration/Migrate consumer groups/Migrate consumer group metadata from a user-created Kafka cluster to Message Queue
         for Apache Kafka.md)
    -   Data migration

        [Migrate data to the cloud](/intl.en-US/User guide/Migration/Migrate data/Migrate data to the cloud.md)

    -   Consumer offset migration

        **Note:** You cannot migrate the consumer offset of a user-created Kafka cluster to Message Queue for Apache Kafka.


## View the migration progress

**Note:** Message Queue for Apache Kafka does not support reporting information about data migration and consumer offset migration. If you have started data migration or consumer offset migration, you cannot view the migration progress in the Message Queue for Apache Kafka console.

To view the migration progress, perform the following steps:

1.  Log on to the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Migration**.

4.  Click the **Migration** tab.

    All cloud migration tasks of the region appear on the **Migration** tab.

    ![Migration progress](../images/p135788.png)

5.  Find the migration task and click **Details**.

    ![Migration progress details](../images/p135789.png)


