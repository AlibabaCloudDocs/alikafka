---
keyword: [kafka, consumer group]
---

# Is automatic creation of consumer groups supported?

Yes, Message Queue for Apache Kafka supports automatic creation of consumer groups.

For more information about how to automatically create consumer groups in Message Queue for Apache Kafka, see [Automatically create consumer groups](/intl.en-US/User guide/Consumer groups/Automatically create consumer groups.md).

Automatic creation of consumer groups facilitates usage but increases the O&M difficulty and causes system instability. In Message Queue for Apache Kafka, consumer groups must be authenticated. We recommend that you create consumer groups by using the following methods:

-   In the console: [Step 2: Create a consumer group](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   By using API operations: [CreateConsumerGroup](/intl.en-US/API reference/Consumer groups/CreateConsumerGroup.md)
-   By using Terraform: [alicloud\_alikafka\_consumer\_group](https://www.terraform.io/docs/providers/alicloud/r/alikafka_consumer_group.html?spm=a2c4g.11186623.2.12.3c0d57bbgZShu7)

