---
keyword: [kafka, callback time, client, java]
---

# Does the callback setting on the Java client affect the message sending speed?

It depends on the processing time in the callback and the setting of max.in.flight.requests.per.connection.

Whether the callback setting on the Java client affects the message sending speed depends on the following aspects:

-   Processing time in the callback: To reduce the processing time in a callback, do not frequently perform time-consuming processing in the callback. You can process multiple callbacks after a specific number of ACKs are accumulated or process them in another asynchronous thread to avoid blocking the completion of callbacks.
-   Setting of max.in.flight.requests.per.connection: Before the blocking ends, the maximum number of messages that can be sent is determined by the setting of max.in.flight.requests.per.connection.

