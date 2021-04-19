---
keyword: [kafka, stability, kernel capability, governance capability]
---

# Comparison between Message Queue for Apache Kafka and open source Apache Kafka

This topic compares Message Queue for Apache Kafka and open source Apache Kafka in terms of stability, kernel capabilities, governance capabilities, and user habits.

## Stability

|Item|Message Queue for Apache Kafka|Apache Kafka|
|----|------------------------------|------------|
|Disk usage|Deletes earlier data when no free disk space is available.|Experiences downtime when no free disk space is available.|
|Thread pool isolation|Ensures normal data write when cold data are read.|Experiences thread blocking when cold data are read. This may cause frequent data write failures.|
|Partition size|Stably writes data to tens of thousands of partitions.|Experiences frequent jitter when data is written to thousands of partitions.|
|Inspection system|Automatically detects and fixes deadlocks and breakdowns.|None.|
|Bug fixes|Detects and fixes bugs at the earliest opportunity.|Waits for the community to release new versions to fix bugs, which takes a long time.|

## Kernel capabilities

|Item|Message Queue for Apache Kafka|Apache Kafka|
|----|------------------------------|------------|
|Scalability|Supports scaling in seconds without affecting the service.|Supports scaling in hours, which affects clusters due to increased replication traffic.|
|Storage cost|Provides highly durable cloud storage in the Professional Edition to save large amounts of storage space.|Provides three-replica storage to ensure availability and durability, which imposes a heavy load on storage.|

## Governance capabilities

|Item|Message Queue for Apache Kafka|Apache Kafka|
|----|------------------------------|------------|
|Version upgrade|Supports one-click upgrade.|Supports manual upgrade, which is prone to errors.|
|Metrics curve|Provides a complete metrics curve to facilitate traffic tracing and troubleshooting.|Provides only real-time metrics. Historical data is difficult to access.|
|Message accumulation alerts|Triggers alerts on message accumulation.|None.|
|Subscription|Provides complete subscriptions.|Provides brief subscriptions.|
|Partition status|Provides a complete partition status diagram.|Provides a brief partition status diagram.|
|Message sending|Allows you to directly send messages in the console.|Allows you to send messages only by running commands on the command line, which results in high costs.|
|Message query|Allows you to directly query messages by time or offset in the console.|Allows you to consume messages by running commands on the command line. However, you cannot view messages by time or offset.|

## User habits

Message Queue for Apache Kafka is consistent with open source Apache Kafka in terms of client protocols. Therefore, you can seamlessly migrate applications and code that are developed based on open source Apache Kafka to Message Queue for Apache Kafka. On the premise that the communication protocols are fully compatible, to provide richer message management and governance features, Message Queue for Apache Kafka imposes the following limits on user habits.

|Item|Message Queue for Apache Kafka|Apache Kafka|Reason for difference|
|----|------------------------------|------------|---------------------|
|**Topic**|
|Creation method|-   The Message Queue for Apache Kafka console
-   Message Queue for Apache Kafka API
-   Kafka CLI \(disabled by default\)
-   Kafka Manager \(disabled by default\)
-   Automatic topic creation on a broker \(disabled by default\)

|-   Kafka CLI
-   Kafka Manager
-   Automatic topic creation on a broker

|By default, Message Queue for Apache Kafka allows you to manage topics by using the console or by calling API operations. Therefore, fine-grained permission control and security management features such as resource actions audit are implemented. **Note:** To create a topic by using Kafka CLI or Kafka Manager or enable a broker to automatically create a topic, submit[a ticket](https://workorder-intl.console.aliyun.com/). |
|Naming conventions|The name can contain uppercase and lowercase letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\), and must be 3 to 64 characters in length.|The name can contain uppercase and lowercase letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\), and must be 3 to 249 characters in length.|If the name of a topic is excessively long, the topic may be limited by other systems during data transmission. Therefore, Message Queue for Apache Kafka limits the length of the topic name.|
|Deletion method|-   The Message Queue for Apache Kafka console
-   Message Queue for Apache Kafka API

|-   Kafka CLI
-   Kafka Manager

|By default, Message Queue for Apache Kafka allows you to manage topics by using the console or by calling API operations. Therefore, fine-grained permission control and security management features such as resource actions audit are implemented. **Note:** Message Queue for Apache Kafka does not allow you to delete a topic by using Kafka CLI or Kafka Manager. |
|**Consumer Group**|
|Creation method|-   The Message Queue for Apache Kafka console
-   Message Queue for Apache Kafka API

|Automatic creation of consumer groups on a broker|By default, Message Queue for Apache Kafka allows you to manage consumer groups by using the console or by calling API operations. Therefore, features such as fine-grained permission control, resource actions audit, and alerts on and monitoring of accumulated messages for consumer groups are implemented. **Note:** To enable a broker to automatically create a consumer group, submit[a ticket](https://workorder-intl.console.aliyun.com/). The preceding features can no longer be used for a consumer group that is automatically created on the broker. |
|Naming conventions|The name can contain uppercase and lowercase letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\), and must be 3 to 64 characters in length.|The name can contain uppercase and lowercase letters, digits, underscores \(\_\), hyphens \(-\), and periods \(.\), and must be 3 to 249 characters in length.|If the name of a consumer group is excessively long, the consumer group may be limited by other systems during data transmission. Therefore, Message Queue for Apache Kafka limits the length of the consumer group name.|
|Deletion method|-   The Message Queue for Apache Kafka console
-   Message Queue for Apache Kafka API

|Kafka CLI|By default, Message Queue for Apache Kafka allows you to manage consumer groups by using the console or by calling API operations. Therefore, fine-grained permission control and security management features such as resource actions audit are implemented. **Note:** Message Queue for Apache Kafka does not allow you to delete a consumer group by using Kafka CLI. |

