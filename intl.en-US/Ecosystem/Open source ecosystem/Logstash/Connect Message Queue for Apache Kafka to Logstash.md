---
keyword: [kafka, logstash]
---

# Connect Message Queue for Apache Kafka to Logstash

This topic describes how to connect Message Queue for Apache Kafka to Logstash.

## Logstash

Logstash is an open-source server-side data processing pipeline that can collect data from multiple sources at the same time, transform it, and store it to the specified location. Logstash processes data in the following way:

1.  Input: collects data of various formats, sizes, and sources. In actual businesses, data is scattered or siloed across multiple systems in various formats. Logstash supports multiple data inputs to collect data from multiple data sources at the same time. Logstash can collect data from logs, web applications, and data storage in a continuous streaming manner.
2.  Filter: parses and transforms data in real time. During data transmission from the source to the destination storage, Logstash filters parse each event, identify named fields to build a structure, and transform them to converge on a common format for more powerful analysis and business value.
3.  Output: exports data. Logstash provides multiple outputs to flexibly adapt to various downstream use cases.

For more information about Logstash, see [Logstash Introduction](https://www.elastic.co/guide/en/logstash/current/introduction.html).

## Connection advantages

Connecting Message Queue for Apache Kafka to Logstash brings the following advantages:

-   Asynchronous processing: improves operating efficiency and prevents burst traffic from affecting user experience.
-   Application decoupling: ensures that when the upstream or downstream application has an exception, the other still runs normally.
-   Overhead reduction: reduces the resource overhead of Logstash.

## Connection methods

Message Queue for Apache Kafka can be connected to Logstash in the following ways:

-   [Connect to Logstash as an input](/intl.en-US/Ecosystem/Open source ecosystem/Logstash/VPC/Connect to Logstash as an input.md)
-   [Connect to Logstash as an output](/intl.en-US/Ecosystem/Open source ecosystem/Logstash/VPC/Connect to Logstash as an output.md)

