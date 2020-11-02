---
keyword: [kafka, connector, 开启]
---

# 开启Connector

本文说明如何为消息队列Kafka版实例开启Connector。

您已购买并部署消息队列Kafka版实例，且该实例必须满足以下条件：

|项目|说明|
|--|--|
|运行状态|服务中|
|版本|消息队列Kafka版实例的版本必须满足以下任意一种要求：-   大版本为0.10.2，且小版本为最新版本。
-   大版本为2.2.0。 |

**说明：** 您可以在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域查看实例的运行状态和版本。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**Connector**。

4.  在**Connector**页面，选择实例，单击**开通Connector**。

5.  在**提示**对话框，单击**确认**。


为消息队列Kafka版实例开启Connector后，您可以创建Connector将消息队列Kafka版实例的数据同步到函数计算或大数据计算服务MaxCompute。

-   [创建FC Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)
-   [创建MaxCompute Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)

