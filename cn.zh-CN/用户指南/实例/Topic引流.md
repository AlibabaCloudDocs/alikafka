---
keyword: [kafka, topic引流, 横向扩容]
---

# Topic引流

您在升级消息队列Kafka版实例的流量规格时，可能会触发集群横向扩容。集群横向扩容完成后，您需要进行Topic引流，使Topic流量重新均匀分布到扩容后的集群上。否则原有的Topic流量还是打在扩容前的集群节点上，原有的Topic的峰值流量会受限于扩容前的峰值流量。新增的Topic不受限于扩容前的流量规格。

您的消息队列Kafka版实例处于**服务中（Topic待引流）**状态。

**说明：** 升级实例的流量规格操作以及集群横向扩容触发规则，请参见[升级实例配置](/cn.zh-CN/用户指南/实例/升级实例配置.md)。

## 注意事项

消息队列Kafka版实例处于**服务中（Topic待引流）**状态时，您可以正常使用该实例收发消息，但不能在该实例下创建Topic、Consumer Group等资源。您必须完成Topic引流或者选择不引流，才能重新创建资源。

## 引流方式

消息队列Kafka版支持的引流方式如下。

|引流方式|原理|影响|适用场景|持续时间|
|----|--|--|----|----|
|所有Topic新增分区|为原集群节点上的所有Topic在扩容后的新节点中增加分区。|-   分区消息乱序。
-   分区数量改变。如果您的客户端无法自动感知到新分区（例如：指定分区发送消费以及一些流计算场景），您可能需要重启或者修改客户端代码。

|-   不要求分区顺序。
-   不指定分区发送。
-   消费方式采取订阅。

|秒级。|
|所有Topic迁移分区（推荐）|-   Local存储：使用kafka-reassign-partitions工具迁移分区数据。
-   云存储：修改映射关系，不迁移分区数据。

|-   Local存储：临时性的内部流量。
-   云存储：无临时性的内部流量。

|任何集群扩容场景。|-   Local存储：分钟级或小时级。 取决于要迁移的Local存储数据量。如果数据量较大，可能持续几小时甚至更久，您需要谨慎评估。建议您在业务流量低峰期执行迁移操作。
-   云存储：秒级。迁移1个Topic大约需要30秒。 |
|不引流（不推荐）|不进行任何操作，即原有的Topic依旧分布在扩容前的集群节点上，新增的Topic均衡分布到扩容后的所有集群节点上。|-   原有的Topic峰值流量会受限于扩容前的流量规格。
-   如果原有的Topic流量较大，可能会出现集群节点之间流量不均衡。

|-   原有的Topic流量非常小，并且集群扩容后原有的Topic流量没有较大提升。
-   集群扩容后会新建Topic，并且绝大部分流量会打在新建的Topic上。

|立即生效。|

## 操作步骤

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在**实例详情**页面的**运行状态**区域，单击**立即引流**。

6.  在**引流方式**对话框，选择引流方式：

    -   所有Topic增加分区

        选择**所有Topic新增分区**，然后单击**确定**。

    -   所有Topic迁移分区
        1.  提交[工单](https://selfservice.console.aliyun.com/#/ticket/category/alikafka/today)联系消息队列Kafka版技术人员将服务端升级至最新版本。
        2.  选择**所有Topic迁移分区**，然后单击**确定**。
    -   不引流

        选择**不引流**，然后单击**确定**。


Topic引流完成后，实例运行状态显示**服务中**。

