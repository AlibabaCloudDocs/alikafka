---
keyword: [kafka, filebeat]
---

# Connect Message Queue for Apache Kafka to Filebeat

This topic describes how to connect Message Queue for Apache Kafka to Filebeat.

## Filebeat

Filebeat is a lightweight shipper for forwarding and centralizing log data. Filebeat can monitor a specified log file or location, collect log events, and forward them to Elasticsearch or Logstash for indexing. Filebeat works in the following way:

1.  Filebeat starts one or more inputs to search for log data in the specified location.
2.  Filebeat starts a harvester for each log that it found. Each harvester reads a single log and sends the log data to libbeat.
3.  libbeat aggregates the data and then sends the aggregated data to the configured output.

## Benefits

The operation to connect Message Queue for Apache Kafka to Filebeat brings the following benefits:

-   Asynchronous processing: prevents burst traffic.
-   Application decoupling: ensures that a downstream exception does not affect the upstream.
-   Overhead reduction: reduces the resource overhead of Filebeat.

## Connection methods

You can connect Message Queue for Apache Kafka to Filebeat by using the following methods:

-   VPC
    -   [Connect to Filebeat as an input](/intl.en-US/Ecosystem/Open source ecosystem/Filebeat/VPC/Connect to Filebeat as an input.md)
    -   [Connect to Filebeat as an output](/intl.en-US/Ecosystem/Open source ecosystem/Filebeat/VPC/Connect to Filebeat as an output.md)
-   Internet
    -   [Connect a Message Queue for Apache Kafka instance to Filebeat as an input]()
    -   [Connect a Message Queue for Apache Kafka instance to Filebeat as an output]()

