# Why do the consumption time points of messages in different partitions vary greatly with an obvious lag or why are they in disorder?

In the Message Queue for Apache Kafka console, when you click **Consumption Status** on the Consumer Group tab, **Last Consumed At** appears, which indicates when the last consumed message was stored in the current partition, rather than the time when the message was consumed. For more information about the preceding operation, see [t998831.md\#](/intl.en-US/User guide/Consumer groups/View the consumption status.md).

If the time indicated by **Last Consumed At** for a partition is earlier than that for other partitions in the console, this partition may receive messages from the producer earlier than other partitions.

Each consumer instance in the same consumer group consumes messages in an evenly divided number of partitions. If the number \(N\) of consumers can be divided by 24 \(the default number of partitions\) with no remainder, each consumer consumes messages in N/24 partitions. Under this condition, whether messages are evenly consumed depends on whether the producer evenly sends messages to each partition.

If the number \(N\) of consumers cannot be divided by 24 \(the default number of partitions\) with no remainder, some consumers may process messages of one more partition than other consumers do. Assume that there are 5 consumers and 24 partitions. Four consumers each consume messages of 5 partitions, and the remaining 1 consumer consumes messages of 4 partitions. The speed of consumption mainly depends on the processing performance of the consumers \(clients\). If all consumers have the same processing performance, the four consumers that each consume messages of five partitions may consume messages more slowly than the consumer that consumes messages of four partitions.

