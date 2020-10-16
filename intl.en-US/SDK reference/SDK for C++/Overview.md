---
keyword: [kafka, c++, send and subscribe to messages]
---

# Overview

A C++ client can connect to Message Queue for Apache Kafka and send and subscribe to messages through various endpoints.

The following table lists the endpoints provided by Message Queue for Apache Kafka.

|Item|Default endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|----------------------------------------------------------|
|Network|VPC|VPC|
|Protocol|PLAINTEXT|SASL\_PLAINTEXT|
|Port|9092|9094|
|SASL mechanism|N/A|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka improves the PLAIN mechanism and allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism, providing higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|[PLAINTEXT](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-cpp-demo/vpc)|None|
|Documentation|[Use the default endpoint to send and subscribe to messages](/intl.en-US/SDK reference/SDK for C++/Use the default endpoint to send and subscribe to messages.md)|None|

