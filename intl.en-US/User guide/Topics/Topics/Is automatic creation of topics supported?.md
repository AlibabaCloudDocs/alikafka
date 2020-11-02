# Is automatic creation of topics supported?

Yes, Message Queue for Apache Kafka supports automatic creation of topics.

For more information about automatic creation of topics in Message Queue for Apache Kafka, see [Automatically create a topic](/intl.en-US/User guide/Topics/Automatically create a Topic.md).

Automatic creation of topics facilitates usage but increases the O&M difficulty and causes system instability. In Message Queue for Apache Kafka, topics must be authenticated. We recommend that you create a topic by using one of the following methods:

-   In the console: [Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   By using API operations: [CreateTopic](/intl.en-US/API reference/Topic/CreateTopic.md)
-   By using Terraform: [alicloud\_alikafka\_topic](https://www.terraform.io/docs/providers/alicloud/r/alikafka_topic.html)

