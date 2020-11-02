---
keyword: [kafka, consumer, automatic creation]
---

# Automatically create consumer groups

You can enable automatic creation of consumer groups in testing or temporary migration scenarios. Do not leave this feature enabled in a production environment for a long time. If you do so, resources may be arbitrarily created due to improper use on the client. In production environments, we recommend that you use the Message Queue for Apache Kafka console or API to manage consumer groups.

The following operations are completed:

1.  Check that the major version of the Message Queue for Apache Kafka instance is 2.x, and the minor version is the latest. For more information about how to check and upgrade the major or minor version of the Message Queue for Apache Kafka instance, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
2.  Submit a [ticket](https://selfservice.console.aliyun.com/ticket/category/alikafka) to enable automatic creation of consumer groups for the Message Queue for Apache Kafka instance.

After you enable automatic creation of consumer groups for a Message Queue for Apache Kafka instance, the Message Queue for Apache Kafka instance creates a consumer group whenever the client requests the metadata of a consumer group that does not exist. For example, if you use a consumer group that does not exist to subscribe to messages, the Message Queue for Apache Kafka instance automatically creates the specified consumer group.

## Call consumer APIs to automatically create consumer groups

After you enable automatic creation of consumer groups for a Message Queue for Apache Kafka instance, you can call consumer APIs to automatically create consumer groups.

**Note:**

-   The names of automatically created consumer groups must follow the naming conventions for consumer groups in Message Queue for Apache Kafka. Otherwise, the system will not create the consumer groups.
-   The number of automatically created consumer groups cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the consumer groups.
-   Automatically created consumer groups are not managed by the Message Queue for Apache Kafka console. Therefore, they do not appear on the **Consumer Groups** page in the Message Queue for Apache Kafka console.

1.  Call the consumer API to automatically create a consumer group.

    The following code provides an example for calling the consumer API to automatically create a consumer group:

    ```
    props.put(ConsumerConfig.GROUP_ID_CONFIG, "newConsumerGroup");
    consumer.subscribe(Collections.singletonList("newTopicName"));
    // If the specified consumer group does not exist, the Message Queue for Apache Kafka broker will automatically create it.
    consumer.poll(Duration.ofSeconds(1));
    ```


