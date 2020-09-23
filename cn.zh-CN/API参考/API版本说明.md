# API版本说明

本文介绍消息队列Kafka版所有API版本的发布信息。

|API版本|发布日期|说明|
|-----|----|--|
|V1.2.4|2019年12月25日|[CreateTopic](/cn.zh-CN/API参考/Topic/CreateTopic.md)新增请求参数PartitionNum。|
|V1.2.3|2019年12月23日|新增以下接口： -   实例
    -   创建后付费实例：[CreatePostPayOrder](/cn.zh-CN/API参考/实例/CreatePostPayOrder.md)
    -   创建预付费实例：[CreatePrePayOrder](/cn.zh-CN/API参考/实例/CreatePrePayOrder.md)
    -   部署实例：[StartInstance](/cn.zh-CN/API参考/实例/StartInstance.md)
    -   升配后付费实例：[UpgradePostPayOrder](/cn.zh-CN/API参考/实例/UpgradePostPayOrder.md)
    -   升配预付费实例：[UpgradePrePayOrder](/cn.zh-CN/API参考/实例/UpgradePrePayOrder.md)
    -   将实例从后付费转为预付费：[ConvertPostPayOrder](/cn.zh-CN/API参考/实例/ConvertPostPayOrder.md)
    -   释放后付费实例：[ReleaseInstance](/cn.zh-CN/API参考/实例/ReleaseInstance.md)
    -   修改实例别名：[ModifyInstanceName](/cn.zh-CN/API参考/实例/ModifyInstanceName.md)
-   Tag
    -   查询资源绑定的标签列表：[ListTagResources](/cn.zh-CN/API参考/标签/ListTagResources.md)
    -   为资源创建并绑定标签：[TagResources](/cn.zh-CN/API参考/标签/TagResources.md)
    -   为资源解绑并删除标签：[UntagResources](/cn.zh-CN/API参考/标签/UntagResources.md) |
|V1.0.3|2018年12月6日|新增以下接口： -   删除Consumer Group：[DeleteConsumerGroup](/cn.zh-CN/API参考/Consumer Group/DeleteConsumerGroup.md)
-   删除Topic：[DeleteTopic](/cn.zh-CN/API参考/Topic/DeleteTopic.md)
-   创建Topic：[CreateTopic](/cn.zh-CN/API参考/Topic/CreateTopic.md)
-   创建ConsumerGroup：[CreateConsumerGroup](/cn.zh-CN/API参考/Consumer Group/CreateConsumerGroup.md) |
|V1.0.1|2018年10月25日|[GetConsumerProgress](/cn.zh-CN/API参考/Consumer Group/GetConsumerProgress.md)的返回值数据结构GetConsumerProgressResponse.ConsumerProgress.TopicListItem.OffsetListItem增加属性分区字段Partition。|
|V1.0.0|2018年10月25日|消息队列Kafka版第一版API提供以下查询类接口： -   获取实例列表：[GetInstanceList](/cn.zh-CN/API参考/实例/GetInstanceList.md)
-   获取Topic列表：[GetTopicList](/cn.zh-CN/API参考/Topic/GetTopicList.md)
-   获取Topic的消息收发状态：[GetTopicStatus](/cn.zh-CN/API参考/Topic/GetTopicStatus.md)
-   获取消费ID列表：[GetConsumerList](/cn.zh-CN/API参考/Consumer Group/GetConsumerList.md)
-   获取消费进度信息：[GetConsumerProgress](/cn.zh-CN/API参考/Consumer Group/GetConsumerProgress.md) |

