---
keyword: [kafka, best practices]
---

# Best practices for subscribers

This topic describes the best practices of Message Queue for Apache Kafka subscribers to help you reduce message consumption errors.

## Basic process of message consumption

The following process describes how Message Queue for Apache Kafka subscribers consume messages:

1.  Poll data.
2.  Execute the consumption logic.
3.  Poll data again.

## Load balancing

Each consumer group can contain multiple consumer instances. This means that you can enable multiple Message Queue for Apache Kafka consumers and set the `group.id` parameter of these consumers to the same value. Consumer instances in the same consumer group consume the subscribed topics in load balancing mode.

For example, Consumer group A has subscribed to Topic A and enabled Consumer instances C1, C2, and C3. Each message sent to Topic A will only be sent to one of C1, C2, and C3. By default, Message Queue for Apache Kafka evenly transfers messages to different consumer instances to balance the consumption loads.

To implement load balancing in consumption, Message Queue for Apache Kafka evenly distributes the partitions of subscribed topics to the consumer instances. Therefore, the number of consumer instances cannot be greater than the number of partitions. Otherwise, some consumer instances may not be assigned any partitions and will be in the dry-run state. Load balancing is triggered not only when consumer instances are started for the first time, but also when consumer instances are restarted, added, or removed.

## Number of partitions

The number of partitions affects the number of concurrent consumer instances.

Messages in one partition can be consumed by only one consumer instance within the same consumer group. Therefore, the number of consumer instances cannot be greater than the number of partitions. Otherwise, some consumer instances may not be assigned any partitions and will be in the dry-run state.

We recommend that you set the number of partitions to a value in the range from 12 to 100. A value less than 12 may affect the message consumption and sending performance. A value greater than 100 may trigger consumer-side rebalancing.

The number of partitions in the console is 12 by default. This can meet the requirements in most scenarios. You can increase the value based on your business needs.

**Note:** Partitions cannot be reduced. Therefore, slightly adjust the number of partitions.

## Multiple subscription modes

Message Queue for Apache Kafka supports the following subscription modes:

-   One consumer group subscribes to multiple topics.

    One consumer group can subscribe to multiple topics. Messages from multiple topics are evenly consumed by consumers in the consumer group. For example, Consumer group A has subscribed to Topic A, Topic B, and Topic C. Messages from the three topics are evenly consumed by consumers in Consumer group A.

    The following code shows an example that one consumer subscribes to multiple topics:

    ```
    String topicStr = kafkaProperties.getProperty("topic");
    String[] topics = topicStr.split(",");
    for (String topic: topics) {
    subscribedTopics.add(topic.trim());
    }
    consumer.subscribe(subscribedTopics);
    ```

-   Multiple consumer groups subscribe to one topic.

    Multiple consumer groups can subscribe to the same topic, and each consumer group separately consumes all messages from the topic. For example, Consumer groups A and B have subscribed to Topic A. Each message sent to Topic A will be transferred to the consumer instances in Consumer group A and the consumer instances in Consumer group B. The two transfer processes are independent of each other without mutual impacts.


## One consumer group for a single application

We recommend that you configure one consumer group for one application. This means that different applications have different pieces of code. If you need to write different pieces of code in the same application, you must prepare multiple different kafka.properties files, such as kafka1.properties and kafka2.properties.

## Consumer offsets

Each topic contains multiple partitions, and each partition counts the total number of current messages, which is the maximum offset MaxOffset.

In Message Queue for Apache Kafka, a consumer consumes messages in a partition in order and records the number of consumed messages, which is the consumer offset ConsumerOffset.

The number of unconsumed messages is calculated by subtracting ConsumerOffset from MaxOffset. This number indicates the message accumulation status.

## Commit consumer offsets

Message Queue for Apache Kafka provides the following parameters for consumers to commit consumer offsets:

-   enable.auto.commit: The default value is true.
-   auto.commit.interval.ms: The default value is 1000, indicating 1 second.

After you set the two parameters, the consumer checks the last time when the consumer offset is committed before each poll. If the interval between this time and the current time exceeds the interval specified by the auto.commit.interval.ms parameter, the consumer commits a consumer offset.

Therefore, if the enable.auto.commit parameter is set to true, you must make sure that all the data polled last time has been consumed before each poll. Otherwise, unconsumed messages may be skipped.

To commit consumer offsets by yourself, set the enable.auto.commit parameter to false and call the commit\(offsets\) function.

## Reset consumer offsets

The consumer offset is reset in the following scenarios:

-   No offset has been committed to the broker, for example, when the consumer is online for the first time.
-   A message is pulled from an invalid offset. For example, the maximum offset in a partition is 10, but consumers start consumption from offset 11.

On the Java client, you can configure the auto.offset.reset parameter to one of the following values:

-   latest: Reset the consumer offset to the maximum offset.
-   earliest: Reset the consumer offset to the minimum offset.
-   none: Do not reset the consumer offset.

**Note:**

-   We recommend that you set this parameter to latest instead of earliest. This prevents consumers from consuming messages from the very beginning due to an invalid offset. This way, the consumers do not need to repeatedly consume messages.
-   If you commit the offset by calling the corresponding function, you can set the parameter to none.

## Pull large messages

Consumers actively pull messages from the broker. When consumers pull large messages, you need to control the pulling speed by modifying the following parameters:

-   max.poll.records: If the size of a message exceeds 1 MB, we recommend that you set this parameter to 1.
-   fetch.max.bytes: Set this parameter to a value that is slightly greater than the size of a single message.
-   max.partition.fetch.bytes: Set this parameter to a value that is slightly greater than the size of a single message.

Pull large messages one by one.

## Message duplication and consumption idempotence

In Message Queue for Apache Kafka, the delivery semantics is at least once. This means that a message is delivered at least once to ensure that the message will not be lost. However, this does not ensure that messages are not duplicated. When a network error occurs or the client restarts, a small number of messages may be repeatedly delivered. If the application consumer is sensitive to message duplication \(for example, in order transactions\), consumption idempotence must be implemented.

Use database applications as an example. You can perform the following operations for idempotence check:

-   When you send a message, pass in a key as a unique sequence ID.
-   When you consume a message, check whether the key has been consumed. If yes, skip the message. If no, consume the message once.

If the application is not sensitive to duplication of a few messages, the idempotence check is not required.

## Consumption failure

Message Queue for Apache Kafka messages are consumed one by one in a partition. If a consumer fails to execute the consumption logic after it receives a message, for example, a message fails to be processed due to dirty data on the application server, you can use the following methods to troubleshoot:

-   Make the system keep trying to execute the consumption logic upon failure. This method may block the consumption thread at the current message, resulting in message accumulation.
-   Message Queue for Apache Kafka is not designed to process failed messages. Therefore, you can print failed messages or store them to a service. For example, you can create a topic that is dedicated to store failed messages. Then, you can regularly check the failed messages, analyze the causes, and take appropriate measures.

## Consumption latency

In Message Queue for Apache Kafka, consumers proactively pull messages from the broker. The latency is low if consumers can consume the data promptly. If the latency is high, first check whether any messages are accumulated, and then increase the consumption speed.

## Consumption blocking and accumulation

Consumption accumulation is the most common issue on the consumer side. It may be caused by the following reasons:

-   Consumption is slower than production. Then, you need to increase the consumption speed. For more information, see [Increase the consumption speed](#section_sod_7b4_znp).
-   The consumer is blocked.

After the consumer receives a message, the consumer executes the consumption logic and usually makes some remote calls. If the consumer waits for the call result at this time, the consumer may keep waiting, causing the consumption process to suspend.

The consumer needs to try to prevent the consumption thread from being blocked. If the consumer waits for the call result, we recommend that you set a timeout period for waiting. This way, the consumption is considered failed if no result is returned after timeout.

## Increase the consumption speed

You can increase the consumption speed in one of the following ways:

-   Add consumer instances.

    You can add consumer instances in a process and ensure that each instance corresponds to one thread. Alternatively, you can deploy multiple consumer instance processes. When the number of consumer instances exceeds the number of partitions, the speed cannot be increased and some consumer instances become idle.

-   Add consumption threads.

    Adding a consumer instance is essentially the same as adding a consumption thread to increase the speed. Therefore, a more important method to improve the performance is to add a consumption thread. You can perform the following basic steps:

    1.  Define a thread pool.
    2.  Poll data.
    3.  Submit data to the thread pool for concurrent processing.
    4.  Poll data again after the concurrent processing result is returned.

## Filter messages

Message Queue for Apache Kafka does not provide any semantics for filtering messages. You can use one of the following methods to filter messages:

-   If you need to filter a few types of messages, you can use multiple topics.
-   If you need to filter many types of messages, we recommend that you filter the messages by businesses on the client.

You can select one of the methods as required or integrate both methods.

## Broadcast a message

Message Queue for Apache Kafka does not provide semantics for broadcasting a message. You can simulate message broadcasting by creating different consumer groups.

## Subscriptions

To facilitate troubleshooting, we recommend that consumer instances in the same consumer group subscribe to the same topics.

