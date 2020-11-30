---
keyword: [kafka, connector, ]
---

# 删除Connector

消息队列Kafka版限制了每个实例的Connector数量。如果您不再需要某个Connector，您可以在消息队列Kafka版控制台删除该Connector。

您已创建以下任意一种Connector：

-   [创建MaxCompute Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)
-   [创建FC Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**Connector**。

4.  在**Connector**页面，选择实例，找到要删除的Connector，在其右侧**操作**列，选择**![icon_more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8046936061/p185678.png)** \> **删除**。

5.  在**删除**对话框，单击**确认**。

    **说明：** 当删除Connector时，系统会同时删除该Connector依赖的5个Topic和2个Consumer Group，无论这些资源当时是自动创建的还是手动创建的。


