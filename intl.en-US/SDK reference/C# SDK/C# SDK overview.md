# C\# SDK overview

A C\# client can connect to Message Queue for Apache Kafka and send and receive messages through various endpoints.

The following table lists the endpoints provided by Message Queue for Apache Kafka.

|Item|Default endpoint|Simple Authentication and Security Layer \(SASL\) endpoint|
|----|----------------|----------------------------------------------------------|
|Network|VPC|VPC|
|Protocol|PLAINTEXT|SASL\_PLAINTEXT|
|Port|9092|9094|
|SASL mechanism|Not applicable|-   PLAIN: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   SCRAM: a username and password verification mechanism that provides higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256. |
|Demo|N/A|N/A|
|Documentation|[Use the default endpoint to send and receive messages](/intl.en-US/SDK reference/C# SDK/Use the default endpoint to send and receive messages.md)|-   [Use the PLAIN mechanism to send and receive messages over an SASL endpoint](/intl.en-US/SDK reference/C# SDK/Use the PLAIN mechanism to send and receive messages over an SASL endpoint.md)
-   [Use the SCRAM mechanism to send and receive messages over an SASL endpoint](/intl.en-US/SDK reference/C# SDK/Use the SCRAM mechanism to send and receive messages over an SASL endpoint.md) |

