---
keyword: [Kafka, consumer, automatic creation]
---

# Automatically create consumer groups

You can enable automatic creation of consumer groups in testing or temporary migration scenarios. We recommend that you do not leave this feature enabled in a production environment for a long time. If you do so, resources may be arbitrarily created due to improper use on the client. In production environments, we recommend that you use the Message Queue for Apache Kafka console or API to manage consumer groups.

You have completed the following operations:

1.  Make sure that you have a Message Queue for Apache Kafka instance of major version 0.10.2 and the latest minor version. For more information about how to check and upgrade versions of the Message Queue for Apache Kafka instance, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
2.  Submit a [ticket](https://workorder-intl.console.aliyun.com/) to enable automatic creation of consumer groups for the Message Queue for Apache Kafka instance.

After you enable automatic creation of consumer groups for a Message Queue for Apache Kafka instance, if the client requests the metadata of a consumer group that does not exist, the Message Queue for Apache Kafka instance creates a consumer group. For example, if you subscribe to messages by using a consumer group that does not exist, the Message Queue for Apache Kafka instance automatically creates the specified consumer group.

## Call the consumer API to automatically create consumer groups

**Note:**

-   The names of automatically created consumer groups must follow the naming conventions for consumer groups in Message Queue for Apache Kafka. Otherwise, the system will not create the consumer groups.
-   The number of automatically created consumer groups cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the consumer groups.
-   You cannot manage the automatically created consumer groups in the Message Queue for Apache Kafka console. Therefore, they are not displayed on the **Consumer Groups** page in the Message Queue for Apache Kafka console.

After you enable automatic creation of consumer groups for a Message Queue for Apache Kafka instance, you can call consumer APIs to automatically create consumer groups.

Sample code:

```
props.put(ConsumerConfig.GROUP_ID_CONFIG, "newConsumerGroup");
consumer.subscribe(Collections.singletonList("newTopicName"));
// If the specified consumer group does not exist, the Message Queue for Apache Kafka broker will automatically create the consumer group.
consumer.poll(Duration.ofSeconds(1));
```

