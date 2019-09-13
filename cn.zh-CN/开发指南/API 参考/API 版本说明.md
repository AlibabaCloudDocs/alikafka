# API 版本说明 {#concept_94524_zh .concept}

本文介绍消息队列 for Apache Kafka 所有的 API 版本发布信息。

详情请见下表。

|API 版本|发布日期|说明|
|------|----|--|
|V1.0.3|2018 年 12 月 6 日| -   新增根据实例 ID 创建 Topic 的接口: CreateTopic
-   新增根据实例 ID 创建 ConsumerGroup 的接口：CreateConsumerGroup

 |
|V1.0.1|2018 年 10 月 25 日|GetConsumerProgress 接口的返回值数据结构 `GetConsumerProgressResponse.ConsumerProgress.TopicListItem.OffsetListItem` 增加属性分区字段“partition”|
|V1.0.0|2018 年 10 月 25 日|消息队列 for Apache Kafka 第一版 API 提供以下查询类接口： -   根据 AccessKeyId/AccessKeySecret 获取的实例列表: GetInstanceList
-   根据实例 ID 获取实例下的 Topic 列表：GetTopicList
-   根据 Topic 获取 Topic 信息：GetTopicStatus
-   根据实例 ID 获取消费 ID 列表: GetConsumerList
-   根据 Consumer ID 获取消费进度信息: GetConsumerProgress

 |

