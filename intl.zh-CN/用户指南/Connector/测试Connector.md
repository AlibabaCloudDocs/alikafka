---
keyword: [kafka, 测试, connector]
---

# 测试Connector

如果您需要测试某个Connector，您可以在消息队列Kafka版控制台向Connector发送测试消息。

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

5.  在**Connector 管理**页面，找到目标Connector，在其右侧**操作**列，单击**测试**。

6.  在**发送消息**面板，发送测试消息。

    -   **发送方式**选择**控制台**。
        1.  在**消息 Key**文本框中输入消息的Key值，例如demo。
        2.  在**消息内容**文本框输入测试的消息内容，例如 \{"key": "test"\}。
        3.  设置**发送到指定分区**，选择是否指定分区。
            1.  单击**是**，在**分区 ID**文本框中输入分区的ID，例如0。如果您需查询分区的ID，请参见[查看分区状态](/intl.zh-CN/用户指南/Topic/查看分区状态.md)。
            2.  单击**否**，不指定分区。
    -   **发送方式**选择**Docker**，执行**运行 Docker 容器生产示例消息**区域的Docker命令，发送消息。
    -   **发送方式**选择**SDK**，根据您的业务需求，选择需要的语言或者框架的SDK以及接入方式，通过SDK发送消息。

