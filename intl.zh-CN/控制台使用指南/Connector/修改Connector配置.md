---
keyword: [connector, kafka, 编辑描述]
---

# 修改Connector配置

成功创建FC Sink Connector后，您可以在消息队列Kafka版控制台更新其配置。

[创建FC Sink Connector](/intl.zh-CN/控制台使用指南/Connector/创建Connector/创建FC Sink Connector.md)

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Connector 管理**。

5.  在**Connector 管理**页面，找到目标Connector，在其**操作**列，选择**更多** \> **修改配置**。

6.  在**修改配置**面板，按需修改以下参数，然后单击**确定**。

    |参数|描述|
    |--|--|
    |**消费线程并发数**|数据源Topic的消费线程并发数。默认值为6。取值说明如下：    -   **1**
    -   **2**
    -   **3**
    -   **6**
    -   **12** |
    |**失败处理**|消息发送失败后，是否继续订阅出现错误的Topic的分区。取值说明如下。    -   **继续订阅**：继续订阅出现错误的Topic的分区，并打印错误日志。
    -   **停止订阅**：停止订阅出现错误的Topic的分区，并打印错误日志。
**说明：**

    -   如何查看日志，请参见[查看Connector日志](/intl.zh-CN/控制台使用指南/Connector/查看Connector日志.md)。
    -   如何根据错误码查找解决方案，请参见[错误码]()。
    -   如需恢复对出现错误的Topic的分区的订阅，您需要[提交工单](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352)联系消息队列Kafka版技术人员。 |
    |**重试次数**|消息发送失败后的重试次数。默认为2。取值范围为1~3。部分导致消息发送失败的错误不支持重试。[错误码]()与是否支持重试的对应关系如下：    -   4XX：除429支持重试外，其余错误码不支持重试。
    -   5XX：支持重试。
**说明：** Connector调用[InvokeFunction]()向函数计算发送消息。 |


修改完成后，在**Connector 管理**页面，找到目标Connector，单击其**操作**列的**详情**。在**Connector的详情**页面，查看到更新后的Connector配置。

