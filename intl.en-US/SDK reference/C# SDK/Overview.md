# Overview

A C\# client can connect to Message Queue for Apache Kafka and send and subscribe to messages by using various endpoints.

The following table lists the endpoints of Message Queue for Apache Kafka.

|Item|Default endpoint|Secure Sockets Layer \(SSL\) endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|-------------------------------------|----------------------------------------------------------|
|Network|VPC|Internet|VPC|
|Protocol|PLAINTEXT|SASL\_SSL|SASL\_PLAINTEXT|
|Port|9092|9093|9094|
|SASL mechanism|N/A|PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism that provides higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|None|None|None|
|Documentation|[Use the default endpoint to send and receive messages](/intl.en-US/SDK reference/C# SDK/Use the default endpoint to send and receive messages.md)|[Send and subscribe to messages by using an SSL endpoint with PLAIN authentication](/intl.en-US/SDK reference/C# SDK/Send and subscribe to messages by using an SSL endpoint with PLAIN authentication.md)|-   [Use the PLAIN mechanism to send and receive messages over an SASL endpoint](/intl.en-US/SDK reference/C# SDK/Use the PLAIN mechanism to send and receive messages over an SASL endpoint.md)
-   [Use the SCRAM mechanism to send and receive messages over an SASL endpoint](/intl.en-US/SDK reference/C# SDK/Use the SCRAM mechanism to send and receive messages over an SASL endpoint.md) |

