---
keyword: [kafka, topic, automatic creation]
---

# Automatically create topics

You can enable automatic creation of topics in testing or temporary migration scenarios. If this feature is enabled in a production environment for a long time, resources may be unexpectedly created due to improper use on the client. In production environments, we recommend that you use the Message Queue for Apache Kafka console or API to manage topics.

You have completed the following operations:

1.  Make sure that you have a Message Queue for Apache Kafka instance of major version 2.X and the latest minor version. For more information about how to check and upgrade the versions of the Message Queue for Apache Kafka instance, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
2.  Submit a [ticket](https://workorder-intl.console.aliyun.com/) to enable automatic creation of topics for the Message Queue for Apache Kafka instance.

After you enable automatic creation of topics for a Message Queue for Apache Kafka instance, if the client requests the metadata of a topic that does not exist, the Message Queue for Apache Kafka instance creates a topic. For example, if you send messages to a topic that does not exist, the Message Queue for Apache Kafka instance automatically creates the specified topic.

## Call producer or consumer APIs to automatically create topics

**Note:**

-   The names of automatically created topics must follow the naming conventions for topics in Message Queue for Apache Kafka. Otherwise, the system will not create the topics, and the producers or consumers will receive error messages, such as `Failed to get metadata`.
-   The number of automatically created topics cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the topics. The producers or consumers will receive error messages, such as `Failed to get metadata`.
-   The total number of partitions for the automatically created topics cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the topics. The producers or consumers will receive error messages, such as `Failed to get metadata`.
-   After you enable automatic creation of topics, pay attention to the quotas for topics and partitions in the Message Queue for Apache Kafka console. Therefore, you can purchase new resources and delete resources that are unnecessary.
-   By default, each automatically created topic meets the following conditions: the storage engine is cloud storage, the number of partitions is 12, and the description is "Auto created by metadata." When you call producer or consumer APIs to automatically create topics, you essentially send requests to obtain the metadata of the topics. If the Message Queue for Apache Kafka broker finds that a requested topic does not exist, it will automatically create the specified topic.

After you enable automatic creation of topics for a Message Queue for Apache Kafka instance, you can call producer or consumer APIs to automatically create topics.

-   The following code shows how to call a producer API operation to automatically create topics:

    ```
    // The API for Java is used in this example. APIs in other programming languages are similar to this.
    ProducerRecord<String, String> kafkaMessage =  new ProducerRecord<String, String>("newTopicName", value);
    // If the topic does not exist, the Message Queue for Apache Kafka broker will automatically create a topic that uses cloud storage and has 12 partitions by default.
    Future metadataFuture = producer.send(kafkaMessage);
    ```

-   The following code shows how to call a consumer API operation to automatically create topics:

    ```
    // The API for Java is used in this example. APIs in other programming languages are similar to this.
    consumer.subscribe(Collections.singletonList("newTopicName"));
    // If the topic does not exist, the Message Queue for Apache Kafka broker will automatically create a topic that uses cloud storage and has 12 partitions by default.
    consumer.poll(Duration.ofSeconds(1));
    ```


The automatically created topics are displayed on the **Topics** page in the Message Queue for Apache Kafka console.

## Call the AdminClient.createTopics\(\) method to create topics

**Note:**

-   The names of created topics must follow the naming conventions for topics in Message Queue for Apache Kafka. Otherwise, the system will not create the topics.
-   The number of created topics cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the topics and will return the following error: `Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max topic count of your instance is xxx, topic runs out.`
-   The total number of partitions in the created topics cannot exceed the limits of the Message Queue for Apache Kafka instance. Otherwise, the system will not create the topics and will return the following error: `Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max partition count of your instance is xxx, partition runs out.`
-   The total number of partitions in a single created topic must be no more than 360. Otherwise, the system will not create the topic and will return the following error: `Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max partition count of a single topic is 360.`
-   If you need to send a creation request for a new topic, you must wait until the previous topic creation request is processed. Otherwise, the system will not create the topic and will return the following error: `org.apache.kafka.common.errors.InvalidTopicException: Try to create topic: topicA. But some other topics are being created, please try again later.`
-   You are not allowed to create topics by using the Assignments method.
-   When you create topics, you are not allowed to specify parameters other than topic name, storage engine, cleanup.policy, and number of partitions. Message Queue for Apache Kafka will automatically provide you with the optimal configuration.
-   By default, each created topic contains the "auto created by admin-client" description. If the configuration contains the `"cleanup.policy":"compact"` setting, the created topic uses local storage. Otherwise, the created topic uses cloud storage.
-   If you have enabled the access control list \(ACL\) feature for a Message Queue for Apache Kafka instance, you are not allowed to create topics by calling the `AdminClient.createTopics()` method.

After you enable automatic creation of topics, you can call the `AdminClient.createTopics()` method to create topics.

Sample code:

```
// The API for Java is used in this example. APIs in other programming languages are similar to this.
CreateTopicsOptions createTopicsOptions = new CreateTopicsOptions();
// We recommend that you set this parameter to a time length that is higher than 20 seconds.
createTopicsOptions.timeoutMs(20000);
// If the topic is used only for validation, you can run the following code. The code will not create a real topic.
// createTopicsOptions.validateOnly(true);
Collection newTopics = new ArrayList<>();

// Create a topic that meets the following conditions: the storage engine is cloud storage, the number of partitions is 12, and the replicationFactor is 1 by default.
NewTopic cloudTopic = new NewTopic("cloudTopic", 12, (short) 1);
newTopics.add(cloudTopic);

// Create a topic that meets the following conditions: the storage engine is cloud storage, the number of partitions is 12, and the replicationFactor is 3 by default.
NewTopic compactLocalTopic = new NewTopic("compactLocalTopic", 12, (short) 3);
Map<String, String> map = new HashMap<>();

// Set cleanup.policy to compact.
map.put(TopicConfig.CLEANUP_POLICY_CONFIG, TopicConfig.CLEANUP_POLICY_COMPACT);
compactLocalTopic.configs(map);
newTopics.add(compactLocalTopic);

// Submit the request.
CreateTopicsResult result = adminClient.createTopics(newTopics, createTopicsOptions);

// Wait until the request for topic creation is processed. No error is returned if the topic is created. An error message is returned if the topic fails to be created or the request times out.
result.all().get();
```

The automatically created topics are displayed on the **Topics** page in the Message Queue for Apache Kafka console.

