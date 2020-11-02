# Why is the total number of topics \(partitions\) limited?

A large number of topics \(partitions\) will greatly reduce the cluster performance and stability.

In Message Queue for Apache Kafka, messages are stored and scheduled by partition. If messages are stored in a large number of partitions, the cluster performance and stability are greatly reduced.

