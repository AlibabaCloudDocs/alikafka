# Best practices for producers

This topic describes best practices of Message Queue for Apache Kafka producers to help you reduce errors when you send messages. The best practices in this topic are written based on a Java client. The basic concepts and ideas are the same for other languages, but the implementation details may be different.

## Send a message

Sample code for sending a message:

```
Future<RecordMetadata> metadataFuture = producer.send(new ProducerRecord<String, String>(
        topic,   // The topic of the message.
        null,   // The partition number. We recommend that you set this parameter to null, and then the producer automatically allocates a partition number.
        System.currentTimeMillis(),   // The timestamp.
        String.valueOf(value.hashCode()),   // The message key.
        value   // The message value.
));
```

For more information about the complete sample code, see [SDK overview](/intl.en-US/SDK reference/SDK overview.md).

## Key and Value fields

Message Queue for Apache Kafka V0.10.2.2 has only two message fields: Key and Value.

-   Key: The ID of the message.
-   Value: The content of the message.

To facilitate tracing, set a unique key for each message. You can track a message by using the key, and print the sending and consumption logs to learn about the sending and consumption of the message.

## Retry upon failure

In a distributed environment, messages occasionally fail to be sent due to network issues. This may occur after a message is sent but ACK failure occurs, or a message fails to be sent.

Message Queue for Apache Kafka uses a virtual IP address \(VIP\) network architecture where idle connections are automatically closed. Therefore, clients that are inactive may receive the error message "Connection reset by peer". We recommend that you resend the message.

You can set the following retry parameters based on your business needs:

-   `retries`: the number of retries. We recommend that you set it to `3`.
-   `retry.backoff.ms`: the interval between retries. We recommend that you set it to `1000`.

## Asynchronous transmission

The methods for sending messages are asynchronous. To obtain the sending result, you can call metadataFuture.get\(timeout, TimeUnit.MILLISECONDS\).

## Thread-safe

The producer is thread-safe and can send messages to all topics. In most cases, one application corresponds to one producer.

## ACKs

ACKs have the following values:

-   `acks=0`: No response is returned from the Message Queue for Apache Kafka broker. In this mode, the performance is high, but the data loss risk is also high.
-   `acks=1`: A response is returned when data is written to the primary Message Queue for Apache Kafka broker. In this mode, the performance and data loss risk are moderate. Data loss may occur if the primary Message Queue for Apache Kafka broker unexpectedly quits.

-   `acks=all`: A response is returned only when data is written to the primary Message Queue for Apache Kafka broker and synchronized to the secondary Message Queue for Apache Kafka brokers. In this mode, the performance is low, but the data is secure. Data loss occurs only if all the primary and secondary Message Queue for Apache Kafka brokers unexpectedly quit.

We recommend that you set `acks=1` in normal cases and set `acks=all` for important services.

## Batch

The basic idea of batch sending is to cache messages in the memory and then send them in batches. Message Queue for Apache Kafka improves the throughput by sending messages in batches. However, this also increases the latency. Therefore, we must weigh these two factors in production. When you build a producer, you must consider the following two parameters:

-   `batch.size`: When the message cache volume sent to each partition reaches this value, a network request is triggered. Then, the Message Queue for Apache Kafka client sends the messages to the Message Queue for Apache Kafka broker. The message volume indicates the total number of bytes in all messages in the batch, instead of the number of messages.
-   `linger.ms`: This parameter specifies the maximum retention duration for each message in the cache. If a message is stored longer than this time limit in the cache, the client immediately sends the message to the Message Queue for Apache Kafka broker without considering `batch.size`.

Therefore, Message Queue for Apache Kafka and `linger.ms` determine when a Message Queue for Apache Kafka client sends a message to the Message Queue for Apache Kafka broker. You can modify the parameter settings based on your business needs.

## OOM

Based on the design of batch sending in Message Queue for Apache Kafka, Message Queue for Apache Kafka caches messages and then sends them in batches. However, if excessive messages are cached, an out of memory \(OOM\) error may occur.

-   `buffer.memory`: When the total size of all cached messages exceeds this value, these messages are sent to the Message Queue for Apache Kafka broker. `batch.size` and `linger.ms` are ignored.
-   `buffer.memory`: The default value is 32 MB, which can ensure sufficient performance for a single producer. However, if you enable multiple producers in the same Java Virtual Machine \(JVM\), an OOM error may occur because each producer may occupy 32 MB of the cache space.
-   In normal cases, you do not need to enable multiple producers during production. To prevent OOM in special scenarios, you must set the `buffer.memory` parameter.

## Partitionally ordered messages

In a partition, messages are stored based on the order in which they are sent, and therefore are ordered.

By default, to improve the availability, Message Queue for Apache Kafka does not guarantee the absolute order of messages in a single partition. A small number of messages become out of order during upgrade or downtime due to failovers, and then the messages in a failed partition are moved to other partitions.

For subscription Message Queue for Apache Kafka instances of the Professional Edition, if your business requires messages to be strictly ordered in a partition, you must select Partitionally Ordered Message for Message Type when you create a topic.

