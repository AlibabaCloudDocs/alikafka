# Overview

A Java client can connect to Message Queue for Apache Kafka through various endpoints and send and receive messages.

The following table lists the endpoints provided by Message Queue for Apache Kafka.

|Item|Default endpoint|SASL endpoint|
|----|----------------|-------------|
|Network|VPC|VPC|
|Protocol|PLAINTEXT|SASL\_PLAINTEXT|
|Port|9092|9094|
|SASL mechanism|N/A|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka improves the PLAIN mechanism and allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism, providing higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|[PLAINTEXT](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc)|-   [SASL\_PLAINTEXT/PLAIN](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094)
-   [SASL\_PLAINTEXT/SCRAM](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094) |
|Topic|[Use the default endpoint to send and subscribe to messages](/intl.en-US/SDK reference/Java SDK/Java SDK.md)|-   [Use the PLAIN mechanism to send and subscribe to messages through an SASL endpoint](/intl.en-US/SDK reference/Java SDK/Use the PLAIN mechanism to send and subscribe to messages through an SASL endpoint.md)
-   [Use the SCRAM mechanism to send and subscribe to messages through an SASL endpoint]() |

