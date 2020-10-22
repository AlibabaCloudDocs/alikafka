# RAM权限策略

消息队列Kafka版的控制台和API的权限管理通过访问控制RAM（Resource Access Management）实现。RAM可以让您避免与其他用户共享阿里云云账号密钥，即AccessKey（包含AccessKey ID和AccessKey Secret），按需为其他用户分配最小权限。

## RAM权限策略

在RAM中，权限策略是用语法结构描述的一组权限的集合，可以精确地描述被授权的资源集、操作集以及授权条件。权限策略是描述权限集的一种简单语言规范，RAM支持的语言规范请参见[权限策略语法和结构](/cn.zh-CN/权限策略管理/权限策略语言/权限策略语法和结构.md)。

在RAM中，权限策略是一种资源实体。消息队列Kafka版支持以下两种类型的权限策略：

-   系统权限策略：统一由阿里云创建，您只能使用不能修改，策略的版本更新由阿里云维护，适用于粗粒度地控制RAM用户权限。
-   自定义权限策略：您可以自主创建、更新和删除，策略的版本更新由您自己维护，适用于细粒度地控制RAM用户权限。

## 系统权限策略

消息队列Kafka版支持以下系统权限策略。

|权限策略名称|说明|
|------|--|
|AliyunKafkaFullAccess|消息队列Kafka版的管理权限，被授予该权限的RAM用户具有等同于阿里云账号的权限，即控制台和API的所有操作权限。|
|AliyunKafkaReadOnlyAccess|消息队列Kafka版的只读权限，被授予该权限的RAM用户只具有阿里云账号所有资源的只读权限，不具有控制台和API的操作权限。|

## 系统权限策略示例

以系统权限策略AliyunKafkaFullAccess为例，被授予该权限的RAM用户具有等同于阿里云账号的权限，即控制台和API的所有操作权限。策略内容如下：

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": "alikafka:*",
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

## 自定义权限策略

消息队列Kafka版支持以下自定义权限策略。

|Action名称|权限说明|是否为只读类权限|
|--------|----|--------|
|ReadOnly|只读所有资源|是|
|ListInstance|查看实例|是|
|StartInstance|部署实例|否|
|UpdateInstance|更新实例配置|否|
|ReleaseInstance|释放实例|否|
|ListTopic|查看Topic|是|
|CreateTopic|创建Topic|否|
|UpdateTopic|更新Topic配置|否|
|DeleteTopic|删除Topic|否|
|ListGroup|查看ConsumerGroup|是|
|CreateGroup|创建ConsumerGroup|否|
|UpdateGroup|更新ConsumerGroup配置|否|
|DeleteGroup|删除ConsumerGroup|否|
|QueryMessage|查询消息|是|
|SendMessage|发送消息|否|
|DownloadMessage|下载消息|是|
|CreateDeployment|创建Connector任务|否|
|DeleteDeployment|删除Connector任务|否|
|ListDeployments|查看Connector任务|是|
|UpdateDeploymentRemark|修改Connector任务说明|否|
|GetDeploymentLog|获取Connector任务运行日志|是|
|EnableAcl|开启ACL|否|
|CreateAcl|创建ACL|否|
|DeleteAcl|删除ACL|否|
|ListAcl|查询ACL|是|
|CreateSaslUser|创建SASL用户|否|
|DeleteSaslUser|删除SASL用户|否|
|ListSaslUser|查询SASL用户|是|

## 自定义权限策略示例

以自己创建的自定义权限策略AliyunKafkaCustomAccess为例，被授予该权限策略的RAM用户只具有实例alikafka\_post-cn-xxx的查看实例、查看Topic、查看Consumer Group、查询消息和下载消息的控制台和API的权限。策略内容如下：

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
              "alikafka:ListInstance",
              "alikafka:ListTopic",
              "alikafka:ListGroup",
              "alikafka:QueryMessage",
              "alikafka:DownloadMessage"
                       ],
            "Resource": "acs:alikafka:*:*:alikafka_post-cn-xxx",
            "Effect": "Allow"
        }
    ]
}
```

