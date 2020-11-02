---
keyword: [kafka, data compression]
---

# Is data compression supported?

Yes. The Message Queue for Apache Kafka broker can send and receive compressed data.

To use this feature, you need to set compression-related parameters on a Message Queue for Apache Kafka client. When you set compression-related parameters on a Message Queue for Apache Kafka client, note the following points:

-   Compression format: Formats such as Snappy, LZ4, and GZIP are supported. The GZIP format consumes a large quantity of CPU resources. Therefore, we recommend that you use Snappy or LZ4.
-   Scenarios: Generally, CPU resources are more expensive than traffic and storage resources. Therefore, we recommend that you use compression only in scenarios that require a high compression ratio, such as logs.
-   CPU consumption: Compression occupies extra CPU resources, more than 20% on average. You can test the extra CPU consumption based on the actual scenario.

