# API概览

本文列举消息队列Kafka版支持的API接口。

## 实例

|API|描述|
|---|--|
|[GetInstanceList](/cn.zh-CN/API参考/实例/GetInstanceList.md)|调用GetInstanceList获取某地域下所购买实例的信息。|
|[CreatePostPayOrder](/cn.zh-CN/API参考/实例/CreatePostPayOrder.md)|调用CreatePostPayOrder创建后付费实例。|
|[StartInstance](/cn.zh-CN/API参考/实例/StartInstance.md)|调用StartInstance部署实例。|
|[UpgradePostPayOrder](/cn.zh-CN/API参考/实例/UpgradePostPayOrder.md)|调用UpgradePostPayOrder升配后付费实例。|
|[ConvertPostPayOrder](/cn.zh-CN/API参考/实例/ConvertPostPayOrder.md)|调用ConvertPostPayOrder将实例从后付费转为预付费。|
|[ModifyInstanceName](/cn.zh-CN/API参考/实例/ModifyInstanceName.md)|调用ModifyInstanceName修改实例的别名。|
|[ReleaseInstance](/cn.zh-CN/API参考/实例/ReleaseInstance.md)|调用ReleaseInstance释放后付费实例。|
|[CreatePrePayOrder](/cn.zh-CN/API参考/实例/CreatePrePayOrder.md)|调用CreatePrePayOrder创建预付费实例。|
|[DeleteInstance](/cn.zh-CN/API参考/实例/DeleteInstance.md)|调用DeleteInstance删除实例。|
|[UpgradePrePayOrder](/cn.zh-CN/API参考/实例/UpgradePrePayOrder.md)|调用UpgradePrePayOrder升配预付费实例。|
|[GetAllowedIpList](/cn.zh-CN/API参考/实例/GetAllowedIpList.md)|调用GetAllowdIpList获取IP白名单。|
|[UpdateAllowedIp](/cn.zh-CN/API参考/实例/UpdateAllowedIp.md)|调用UpdateAllowedIp修改IP白名单。|

## Topic

|API|描述|
|---|--|
|[CreateTopic](/cn.zh-CN/API参考/Topic/CreateTopic.md)|调用CreateTopic创建Topic。|
|[DeleteTopic](/cn.zh-CN/API参考/Topic/DeleteTopic.md)|调用DeleteTopic删除Topic。|
|[GetTopicList](/cn.zh-CN/API参考/Topic/GetTopicList.md)|调用GetTopicList获取Topic的列表。|
|[GetTopicStatus](/cn.zh-CN/API参考/Topic/GetTopicStatus.md)|调用GetTopicStatus获取Topic的消息收发状态。|
|[ModifyPartitionNum](/cn.zh-CN/API参考/Topic/ModifyPartitionNum.md)|调用ModifyPartitionNum修改Topic的分区数。|
|[ModifyTopicRemark](/cn.zh-CN/API参考/Topic/ModifyTopicRemark.md)|调用ModifyTopicRemark修改Topic的备注。|

## Consumer Group

|API|描述|
|---|--|
|[CreateConsumerGroup](/cn.zh-CN/API参考/Consumer Group/CreateConsumerGroup.md)|调用CreateConsumerGroup创建ConsumerGroup。|
|[DeleteConsumerGroup](/cn.zh-CN/API参考/Consumer Group/DeleteConsumerGroup.md)|调用DeleteConsumerGroup删除ConsumerGroup。|
|[GetConsumerList](/cn.zh-CN/API参考/Consumer Group/GetConsumerList.md)|调用GetConsumerList获取Consumer Group的列表。|
|[GetConsumerProgress](/cn.zh-CN/API参考/Consumer Group/GetConsumerProgress.md)|调用GetConsumerProgress查询Consumer Group的消费状态。|

## 标签

|API|描述|
|---|--|
|[ListTagResources](/cn.zh-CN/API参考/标签/ListTagResources.md)|调用ListTagResources查询资源绑定的标签信息。|
|[TagResources](/cn.zh-CN/API参考/标签/TagResources.md)|调用TagResources为资源创建并绑定标签。|
|[UntagResources](/cn.zh-CN/API参考/标签/UntagResources.md)|调用UntagResources为资源解绑并删除标签。|

## SASL ACL

|API|描述|
|---|--|
|[DescribeAcls](/cn.zh-CN/API参考/SASL ACL/DescribeAcls.md)|调用DescribeAcls查询ACL。|
|[CreateAcl](/cn.zh-CN/API参考/SASL ACL/CreateAcl.md)|调用CreateAcl创建ACL。|
|[DeleteAcl](/cn.zh-CN/API参考/SASL ACL/DeleteAcl.md)|调用DeleteAcl删除ACL。|

## SASL用户

|API|描述|
|---|--|
|[DescribeSaslUsers](/cn.zh-CN/API参考/SASL用户/DescribeSaslUsers.md)|调用DescribeSaslUsers查询SASL用户。|
|[CreateSaslUser](/cn.zh-CN/API参考/SASL用户/CreateSaslUser.md)|调用CreateSaslUser创建SASL用户。|
|[DeleteSaslUser](/cn.zh-CN/API参考/SASL用户/DeleteSaslUser.md)|调用DeleteSasalUser删除SASL用户。|

