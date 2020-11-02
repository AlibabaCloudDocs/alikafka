# Why is automatic creation of consumer groups not supported?

Automatic creation of consumer groups facilitates usage but increases the O&M difficulty and easily causes system instability. In Message Queue for Apache Kafka, consumer groups must be authenticated. Therefore, Message Queue for Apache Kafka does not automatically create consumer groups. However, you can create consumer groups by using the console, API operations, or automated orchestration tools.

-   In the console: [Step 2: Create a consumer group](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   By using API operations: [CreateConsumerGroup](/intl.en-US/API reference/Consumer groups/CreateConsumerGroup.md)
-   By using Terraform: [alicloud\_alikafka\_consumer\_group](https://www.terraform.io/docs/providers/alicloud/r/alikafka_consumer_group.html?spm=a2c4g.11186623.2.12.3c0d57bbgZShu7)

