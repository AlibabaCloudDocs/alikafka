# Overview

Java clients can send and subscribe to messages by connecting to a Message Queue for Apache Kafka endpoint.

The following table lists the endpoints provided by Message Queue for Apache Kafka.

|Item|Default endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|----------------------------------------------------------|
|Network|VPC|VPC|
|Protocol|PLAINTEXT|SASL\_PLAINTEXT|
|Port|9092|9094|
|SASL mechanism|N/A|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism with higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|[PLAINTEXT](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc)|-   [SASL\_PLAINTEXT/PLAIN](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094)
-   [SASL\_PLAINTEXT/SCRAM](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094) |
|Documentation|[Send and subscribe to messages over the default endpoint](/intl.en-US/SDK reference/SDK/Use the default endpoint to send and subscribe to messages.md)|-   [Send and subscribe to messages over an SASL endpoint with PLAIN authentication](/intl.en-US/SDK reference/SDK/Use the PLAIN mechanism to send and subscribe to messages through an SASL endpoint.md)
-   [Send and subscribe to messages over an SASL endpoint with SCRAM authentication](/intl.en-US/SDK reference/SDK/Use the SCRAM mechanism to send and subscribe to messages through an SASL endpoint.md) |

