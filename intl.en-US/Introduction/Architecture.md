# Architecture

This topic describes the architecture of Message Queue for Apache Kafka and the publish/subscribe pattern.

## Message Queue for Apache Kafka architecture

A Message Queue for Apache Kafka cluster consists of producers, brokers, consumer groups, and ZooKeeper, as shown in [Figure 1](#fig_h4w_luj_jjq).

![Architecture](../images/p129320.png "Message Queue for Apache Kafka architecture")

-   **Producer**

    A producer sends messages to Message Queue for Apache Kafka brokers in push mode. The messages sent can be page views, server logs, and information related to system resources such as CPU utilization and memory usage.

-   **Kafka Broker**

    A broker is a server used to store messages. Brokers can be scaled out. The larger the number of brokers is, the higher the throughput of the Message Queue for Apache Kafka cluster is.

-   **Consumer Group**

    A consumer group subscribes to and consumes messages from a Message Queue for Apache Kafka broker in pull mode.

-   **Zookeeper**

    ZooKeeper manages the cluster configuration, elects the leader partition, and balances loads when a consumer group changes.


## Publish/subscribe pattern of Message Queue for Apache Kafka

Message Queue for Apache Kafka uses the publish/subscribe pattern, as shown in [Figure 2](#fig_clp_z8q_239).

![Publish/subscribe pattern](../images/p129319.png "Publish/subscribe pattern of Message Queue for Apache Kafka")

-   The relationship between consumer groups and topics is N:N. One consumer group can subscribe to multiple topics, and one topic can be subscribed to by multiple consumer groups.
-   Messages in a topic can be consumed only by one consumer in the same consumer group.

