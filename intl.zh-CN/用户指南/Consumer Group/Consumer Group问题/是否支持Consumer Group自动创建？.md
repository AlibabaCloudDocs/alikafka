---
keyword: [kafka, consumer group]
---

# 是否支持Consumer Group自动创建？

消息队列Kafka版部分支持Consumer Group自动创建。

消息队列Kafka版Consumer Group自动创建请参见[自动创建Consumer Group](/intl.zh-CN/用户指南/Consumer Group/自动创建Consumer Group.md)。

Consumer Group自动创建，使用起来方便，运维起来却极其麻烦，且极易造成系统不稳定。消息队列Kafka版的Consumer Group，还涉及一系列鉴权问题。建议您通过以下方式创建Consumer Group：

-   控制台：[步骤二：创建Consumer Group](/intl.zh-CN/快速入门/步骤三：创建资源.md)
-   API：[CreateConsumerGroup](/intl.zh-CN/API参考/Consumer Group/CreateConsumerGroup.md)
-   Terraform：[alicloud\_alikafka\_consumer\_group](https://www.terraform.io/docs/providers/alicloud/r/alikafka_consumer_group.html?spm=a2c4g.11186623.2.12.3c0d57bbgZShu7)

