---
keyword: [connector, kafka, 编辑描述]
---

# 修改Connector配置

成功创建FC Sink Connector后，您可以在消息队列Kafka版控制台更新其配置。

[创建FC Sink Connector](/intl.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Connector（公测组件）**。

6.  在**Connector（公测组件）**页面，找到要修改配置的FC Sink Connector，在其**操作**列，单击![icon_more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8046936061/p185678.png)图标，然后在下拉列表中，选择**修改配置**。

7.  在**修改Connector**面板，按需修改以下配置，然后单击**确定**。

    |参数|描述|
    |--|--|
    |消费线程并发数|数据源Topic的消费线程并发数。默认值为3。取值：    -   3
    -   6
    -   9
    -   12 |
    |失败重试|消息发送失败后的错误处理。默认为log。取值：    -   log：继续对出现错误的Topic的分区的订阅，并打印错误日志。出现错误后，您可以通过Connector日志查看错误，并根据错误的错误码查找解决方案，以进行自助排查。

**说明：**

        -   如何查看Connector日志，请参见[查看Connector日志](/intl.zh-CN/用户指南/Connector/查看Connector日志.md)。
        -   如何根据错误码查找解决方案，请参见[错误码]()。
    -   fail：停止对出现错误的Topic的分区的订阅，并打印错误日志。出现错误后，您可以通过Connector日志查看错误，并根据错误的错误码查找解决方案，以进行自助排查。

**说明：**

        -   如何查看Connector日志，请参见[查看Connector日志](/intl.zh-CN/用户指南/Connector/查看Connector日志.md)。
        -   如何根据错误码查找解决方案，请参见[错误码]()。
        -   如需恢复对出现错误的Topic的分区的订阅，您需要提交[提交工单](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352)联系消息队列Kafka版技术人员。 |
    |重试次数|消息发送失败后的重试次数。默认为2。取值范围为1~3。部分导致消息发送失败的错误不支持重试。错误码与是否支持重试的对应关系如下：    -   4XX：除429支持重试外，其余错误码不支持重试。
    -   5XX：支持重试。
**说明：**

    -   关于错误码的更多信息，请参见[错误码]()。
    -   Connector调用InvokeFunction向函数计算发送消息。关于InvokeFunction的更多信息，请参见[API概览]()。 |


修改完成后，可以在**查看任务配置**面板查看到更新后的Connector配置。

