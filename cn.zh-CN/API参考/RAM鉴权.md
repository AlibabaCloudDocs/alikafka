# RAM鉴权

在使用RAM用户调用消息队列Kafka版API前，需要阿里云账号通过创建授权策略RAM用户进行授权。在授权策略中，使用资源描述符ARN（Alibaba Cloud Resource Name）指定授权资源。

## 可授权的消息队列Kafka版类型

在进行RAM用户授权时，消息队列Kafka版资源的描述方式如下：

|资源类型|授权策略中的资源描述方法|
|----|------------|
|Instance|acs:alikafka:\*:\*:instanceid|

其中`instanceid`为具体的资源ID，`*`代表对应的所有资源。

## 可授权的消息队列Kafka版接口

下表列举了消息队列Kafka版中可授权的API及其描述方式：

|API|资源描述|
|---|----|
|CreatePostPayOrder|acs:alikafka:\*:\*:instanceid|
|GetInstanceList|acs:alikafka:\*:\*:instanceid|
|StartInstance|acs:alikafka:\*:\*:instanceid|
|UpgradePostPayOrder|acs:alikafka:\*:\*:instanceid|
|ConvertPostPayOrder|acs:alikafka:\*:\*:instanceid|
|ModifyInstanceName|acs:alikafka:\*:\*:instanceid|
|ReleaseInstance|acs:alikafka:\*:\*:instanceid|
|ListTopic|acs:alikafka:\*:\*:instanceid|
|CreatePrePayOrder|acs:alikafka:\*:\*:instanceid|
|DeleteInstance|acs:alikafka:\*:\*:instanceid|
|UpgradePrePayOrder|acs:alikafka:\*:\*:instanceid|
|GetAllowdIpList|acs:alikafka:\*:\*:instanceid|
|UpdateAllowedIp|acs:alikafka:\*:\*:instanceid|
|CreateTopic|acs:alikafka:\*:\*:instanceid|
|GetTopicList|acs:alikafka:\*:\*:instanceid|
|DeleteTopic|acs:alikafka:\*:\*:instanceid|
|GetTopicStatus|acs:alikafka:\*:\*:instanceid|
|CreateConsumerGroup|acs:alikafka:\*:\*:instanceid|
|DeleteConsumerGroup|acs:alikafka:\*:\*:instanceid|
|GetConsumerList|acs:alikafka:\*:\*:instanceid|
|GetConsumerProgress|acs:alikafka:\*:\*:instanceid|
|ListTagResources|acs:alikafka:\*:\*:instanceid|
|TagResources|acs:alikafka:\*:\*:instanceid|
|UntagResources|acs:alikafka:\*:\*:instanceid|

