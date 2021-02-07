---
keyword: [kafka, python, send and subscribe to messages]
---

# Overview

A Python client can connect to Message Queue for Apache Kafka and send and subscribe to messages by using various endpoints.

The following table lists the endpoints of Message Queue for Apache Kafka.

|Item|Default endpoint|Secure Sockets Layer \(SSL\) endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|-------------------------------------|----------------------------------------------------------|
|Network|VPC|Internet|VPC|
|Protocol|PLAINTEXT|SASL\_SSL|SASL\_PLAINTEXT|
|Port|9092|9093|9094|
|SASL mechanism|N/A|PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism that provides higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|[PLAINTEXT](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-python-demo/vpc)|[SASL\_SSL/PLAIN](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-python-demo/vpc-ssl)|None|
|Documentation|[Send and subscribe to messages by using the default endpoint](/intl.en-US/SDK reference/SDK for Python/Send and subscribe to messages by using the default endpoint.md)|[Send and subscribe to messages by using an SSL endpoint with PLAIN authentication](/intl.en-US/SDK reference/SDK for Python/Send and subscribe to messages by using an SSL endpoint with PLAIN authentication.md)|None|

