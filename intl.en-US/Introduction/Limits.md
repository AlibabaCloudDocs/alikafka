# Limits

Message Queue for Apache Kafka sets constraints for some metrics. To avoid program exceptions when you use Message Queue for Apache Kafka, do not exceed the maximum limits. The following table lists these limits.

|Item|Limit|Description|
|----|-----|-----------|
|Auto creation of topics and consumer groups|Supported**Note:** To enable auto creation of topics and consumer groups,[submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352).

|Auto creation of topics and consumer groups facilitates usage but increases the O&M difficulty and may cause system instability. In Message Queue for Apache Kafka, topics and consumer groups must be authenticated. We recommend that you create topics and consumer groups by using the following methods:-   Console
    -   [Create topics](/intl.en-US/Quick-start/Step 3: Create resources.md)
    -   [Create consumer groups](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   API
    -   [CreateTopic](/intl.en-US/API reference/Topics/CreateTopic.md)
    -   [CreateConsumerGroup](/intl.en-US/API reference/Consumer groups/CreateConsumerGroup.md)
-   Terraform
    -   [CreateTopic](https://www.terraform.io/docs/providers/alicloud/r/alikafka_topic.html)
    -   [CreateConsumerGroup](https://www.terraform.io/docs/providers/alicloud/r/alikafka_consumer_group.html) |
|Total topic \(partition\) quantity limit|Supported|In Message Queue for Apache Kafka, messages are stored and scheduled by partition. If messages are stored in a large number of topics \(partitions\), storage fragmentation occurs, which reduces the cluster performance and stability.|
|Reduction in the number of partitions of a topic|Not supported|This is due to design constraints of Message Queue for Apache Kafka.|
|Exposed ZooKeeper|Not supported|ZooKeeper has been masked since Message Queue for Apache Kafka 0.9.0. Therefore, you do not need to access ZooKeeper to use the client. In Message Queue for Apache Kafka, ZooKeeper is partially shared. For security purposes, it is not exposed. You do not need to learn about ZooKeeper.|
|Topic-based authentication|Not supported|Topic-based authentication relies on ZooKeeper. Message Queue for Apache Kafka is deployed in virtual private clouds \(VPCs\) and provides sufficient security protection with the security group and whitelist features.|
|Logon to the machines on which Message Queue for Apache Kafka is deployed|Not supported|None|
|Version|-   Message Queue for Apache Kafka Standard Edition

Only version 0.10.x is supported and it is deployed by default.

-   Message Queue for Apache Kafka Professional Edition

Versions 0.10.x to 2.x are supported. Version 0.10.x is deployed by default.


|-   Version 2.x is compatible with versions 0.10.x and 0.9.0.
-   Version 0.10.x is compatible with version 0.9.0.
-   To upgrade a Standard Edition instance from 0.10.x to 2.x, you must first upgrade the instance to the Professional Edition and then upgrade the open-source version of the instance to 2.x. For more information, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade instance specifications.md) and [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).
-   To upgrade a Professional Edition instance from 0.10.x to 2.x, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md). |
|Consumer group count|The consumer group count is twice the topic count.|For example, if an instance has 50 topics, you can create up to 100 consumer groups in this instance. If you need to increase the number of consumer groups, you can increase the number of topics. The number of consumer groups is increased by two each time a topic is added. For more information, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).|
|Relationship between the topic count and partition count|1:16|In addition to the default number of partitions, 16 partitions are added for each additional topic. For example, assume that you have purchased a Standard Edition \(High Write\) instance with 50 topics, 2xlarge traffic, and 400 partitions by default. If you purchase another 10 topics for this instance, 160 additional partitions are added to this instance. The total number of partitions becomes 560.|
|Number of topics of a Professional Edition instance|The number of topics of a Professional Edition instance is twice the number of purchased topics.|For example, if you purchase a Professional Edition instance with 50 topics, you can create 100 topics in the instance.|
|Change of the region or network properties of an instance|Not supported|After you have purchased and deployed an instance, its region and network properties are closely associated with its physical resources and cannot be changed. If you need to change the region or network properties of an instance, release the instance and purchase a new instance.|
|Message size|10 MB|The maximum size of a message is 10 MB. A message larger than 10 MB cannot be sent.|
