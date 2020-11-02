---
keyword: [kafka, tcp connection, producer, broker]
---

# How many connections to the broker can a producer establish?

Each producer usually establishes two TCP connections to the broker.

Each producer usually establishes two TCP connections to the broker. One TCP connection is used to update metadata and the other is used to send messages.

For more information, see [How TCP Connections are Managed by kafka-clients scala library](http://stackoverflow.com/questions/47936073/how-are-tcp-connections-managed-by-kafka-clients-scala-library?rq=1).

