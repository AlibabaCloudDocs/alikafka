---
keyword: [kafka, connector, ]
---

# 删除Connector

消息队列Kafka版限制了每个实例的Connector数量。如果您不再需要某个Connector，您可以在消息队列Kafka版控制台删除该Connector。

您已创建以下任意一种Connector：

-   [创建FC Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)
-   [创建MaxCompute Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)
-   [创建OSS Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建OSS Sink Connector.md)
-   [创建Elasticsearch Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建Elasticsearch Sink Connector.md)
-   [创建MySQL Source Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建MySQL Source Connector.md)

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Connector 管理**。

5.  在**Connector 管理**页面，找到目标Connector，在其**操作**列，选择**更多** \> **删除**。

6.  在**提示**对话框，单击**确认**，删除Connector。

    **说明：**

    -   如果MySQL Source Connector任务处于运行状态，在消息队列Kafka版控制台将无法直接删除，您需登录DataWorks控制台停止并下线Connector任务，然后联系消息队列Kafka版值班号清理消息队列Kafka版Connector任务的元信息。其他FC Sink Connector、MaxCompute Sink Connector、OSS Sink Connector以及Elasticsearch Sink Connector任务，均可在消息队列Kafka版控制台直接删除。
    -   当删除Connector时，系统会同时删除该Connector依赖的5个Topic和2个Consumer Group，无论这些资源当时是自动创建的还是手动创建的。

