# Best practices for producers

This topic describes the best practices of Message Queue for Apache Kafka producers to help you reduce errors when messages are sent. The best practices in this topic are written for a Java client. The basic concepts and ideas are the same for other languages, but the implementation details may be different.

## Send a message

Sample code for sending a message:

```
Future<RecordMetadata> metadataFuture = producer.send(new ProducerRecord<String, String>(
        topic,   // The topic of the message.
        null,   // The partition number. The recommended value is null, which indicates that the partition is assigned by the producer.
        System.currentTimeMillis(),   // The timestamp.
        String.valueOf(value.hashCode()), // The message key.
        value   // The message value.
));
```

For the complete sample code, see [t998844.md\#](/intl.en-US/SDK reference/Overview.md).

## Keys and values

Message Queue for Apache Kafka 0.10.2.2 has only two message fields: Key and Value.

-   Key: the ID of the message.
-   Value: the content of the message.

To facilitate tracing, set a unique key for each message. You can track a message based on the key and print logs to see details about the sending and consumption of the message.

## Retry upon failure

In a distributed environment, messages occasionally fail to be sent due to network issues. This may occur when a message has been sent but ACK failure occurs, or a message fails to be sent.

Message Queue for Apache Kafka uses a virtual IP \(VIP\) network architecture where connections that are inactive for 30 seconds are automatically terminated. Therefore, clients that are not always active often receive the error message "Connection reset by peer". In this case, we recommend that you resend the message.

You can set the following retry parameters as required:

-   `retries`: the number of retries. We recommend that you set the value to `3`.
-   `retry.backoff.ms`: the retry interval. We recommend that you set it to `1000`.

## Asynchronous transmission

The sending API is asynchronous. To obtain the sending result, you can call metadataFuture.get\(timeout, TimeUnit.MILLISECONDS\).

## Thread safety

The producer is thread-safe and can send messages to all topics. Generally, one application corresponds to one producer.

## Acks

Acks have the following values:

-   `acks=0`: The broker does not return responses. In this mode, performance is high, but the risk of data loss is also high.
-   `acks=1`: The primary broker returns a response when data is written to the broker. In this mode, both performance and the risk of data loss are moderate. Data loss may occur if the primary broker quits unexpectedly.

-   `acks=all`: The primary broker returns a response only after data is written to the primary broker and synchronized to the secondary brokers. In this mode, performance is low, but the risk of data loss is negligible. Data loss occurs only if all primary and secondary brokers quit unexpectedly.

We recommend that you set `acks=1` in normal cases and set `acks=all` for important services.

## Batch sending

The basic idea of batch sending is to cache messages in memory and then send them in batches. In Message Queue for Apache Kafka, batch sending improves throughput but also increases latency. These two factors must be considered for production environments. When you build a producer, you must take the following two parameters into account:

-   `batch.size`: When the message cache volume sent to each partition reaches this value, a network request is triggered. Then the client sends the messages to the server. Note that the value of batch.size is the total number of bytes in all messages in the batch, and not the number of entries.
-   `linger.ms`: This parameter specifies the maximum storage duration for messages in the cache. If a message remains in the cache longer than this time limit, the client immediately sends the message to the broker without considering `batch.size`.

Therefore, `batch.size` and `linger.ms` determine when a Message Queue for Apache Kafka client sends a message to the broker. You can modify the values of these parameters based on your business requirements.

## OOM errors

Message Queue for Apache Kafka caches messages and then sends them in batches. However, if an excessive number of messages are cached, an out of memory \(OOM\) error may occur.

-   `buffer.memory`: If the total size of all cached messages exceeds this value, the messages will be sent to the broker. In this case, `batch.size` and `linger.ms` are ignored.
-   `buffer.memory`: The default value is 32 MB, which can ensure sufficient performance for a single producer. However, if you enable multiple producers in the same Java Virtual Machine \(JVM\), an OOM error may occur because each producer may occupy 32 MB of the cache space.
-   Generally, multiple producers are not used in production environments. If you require multiple producers, you must set the `buffer.memory` parameter to prevent OOM errors.

## Partitionally ordered messages

In each partition, messages are stored in the order that they are sent.

To improve availability, Message Queue for Apache Kafka does not guarantee the absolute order of messages in a single partition by default. A small number of messages can become out of order due to an upgrade or a failover. If a partition fails, messages in that partition are moved to other partitions.

For Message Queue for Apache Kafka Professional Edition instances using subscription billing, if your business requires messages to be strictly ordered in a partition, select Partitionally Ordered Message as the message type when you create a topic.

