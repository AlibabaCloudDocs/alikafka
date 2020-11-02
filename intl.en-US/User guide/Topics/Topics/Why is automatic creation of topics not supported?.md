# Why is automatic creation of topics not supported?

Automatic creation of topics facilitates usage but increases the O&M difficulty and easily causes system instability. In Message Queue for Apache Kafka, topics must be authenticated. Therefore, Message Queue for Apache Kafka does not automatically create topics. However, you can create topics and consumer groups by using the console, API operations, or automated orchestration tools.

-   In the console: [Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   By using API operations: [CreateTopic](/intl.en-US/API reference/Topics/CreateTopic.md)
-   By using Terraform: [alicloud\_alikafka\_topic](https://www.terraform.io/docs/providers/alicloud/r/alikafka_topic.html)

