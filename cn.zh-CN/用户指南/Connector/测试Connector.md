---
keyword: [kafka, 测试, connector]
---

# 测试Connector

如果您需要测试某个Connector，您可以在消息队列Kafka版控制台向Connector发送测试消息。

您已创建以下任意一种Connector：

-   [创建MaxCompute Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)
-   [创建FC Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**Connector**。

4.  在**Connector**页面，选择实例，找到要测试的Connector，在其右侧**操作**列，单击**测试**。

5.  在**提示**对话框，单击**确定**。

6.  在**Topic管理**页面，找到数据源Topic，在其右侧**操作**列，单击**发送消息**。

7.  在**发送消息**对话框，发送测试消息。

    1.  在**分区**文本框，输入0。

    2.  在**Message Key**文本框，输入1。

    3.  在**Message Value**文本框，输入1。

    4.  单击**发送**。


