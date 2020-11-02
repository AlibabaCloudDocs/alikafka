# Terms

This topic describes related terms of Message Queue for Apache Kafka to help you better understand and use Message Queue for Apache Kafka.

## B

-   **Broker**

    An independent server in a Message Queue for Apache Kafka cluster.


## C

-   **Consumer**

    The message subscriber, also known as the message consumer. It reads messages from the Message Queue for Apache Kafka broker and consumes the messages.

-   **Consumer Group**

    A group of consumers that subscribe to and consume messages of the same type based on the same consumption logic. The relationship between consumer groups and topics is N:N. One consumer group can subscribe to multiple topics, and one topic can be subscribed to by multiple consumer groups.


## F

-   **Partitionally ordered messages**

    By default, messages of the same key are stored in the same partition in the order they are sent. When an instance in the cluster fails, the messages are still in order. However, some messages in the partition cannot be sent until the failed instance is restored.


## L

-   **Local storage**

    Based on the In-Sync Replicas \(ISR\) algorithm of Apache Kafka, Message Queue for Apache Kafka stores three replicas in distributed mode and `min.insync.replicas` is set to 2.


## P

-   **Partition**

    A physical partition. Each topic has one or more partitions.

-   **Producer**

    The message publisher, also known as the message producer. It generates and sends messages to the Message Queue for Apache Kafka broker.

-   **Normal messages**

    By default, messages of the same key are stored in the same partition in the order they are sent. When an instance in the cluster fails, the messages may be out of order.


## T

-   **Topic**

    A message classifier. Message Queue for Apache Kafka classifies messages by topic. A topic consists of one or more partitions and is stored on one or more brokers.


## Y

-   **Cloud storage**

    A way to store messages. The underlying layer is connected to multiple replicas of Alibaba Cloud disks. In Message Queue for Apache Kafka, each partition only needs one replica.


