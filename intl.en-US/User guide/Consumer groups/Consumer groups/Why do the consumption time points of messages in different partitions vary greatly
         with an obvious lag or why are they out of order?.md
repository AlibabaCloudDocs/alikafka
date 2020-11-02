# Why do the consumption time points of messages in different partitions vary greatly with an obvious lag or why are they out of order?

Messages are unevenly distributed to consumers in different partitions.

## Symptom

In the Message Queue for Apache Kafka console, when you click **Consumption Status** on the Consumer Group tab, **Last Consumed At** appears. If the time indicated for a partition is earlier than that for other partitions, this partition may receive messages from the producer earlier than other partitions.

## Cause

Each consumer instance in the same consumer group consumes messages in an evenly divided number of partitions. Under this condition, whether messages are evenly consumed depends on whether the producer evenly sends messages to each partition.

-   If the number \(N\) of consumers can be divided by 24 \(the default number of partitions\) without a remainder, each consumer consumes messages in N/24 partitions.
-   If the number \(N\) of consumers cannot be divided by 24 \(the default number of partitions\) without a remainder, some consumers may process messages of one more partition than other consumers do.

    Assume that 5 consumers and 24 partitions are available. Four consumers each consume messages of 5 partitions, and the remaining 1 consumer consumes messages of 4 partitions. The speed of consumption depends on the processing performance of the consumers. If all consumers have the same processing performance, the four consumers that each consume messages of five partitions may consume messages more slowly than the consumer that consumes messages of four partitions.


## Solution

Ensure that the number \(N\) of consumers can be divided by 24 \(the default number of partitions\) without a remainder.

