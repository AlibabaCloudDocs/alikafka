# 创建消息队列Kafka版到OSS的数据同步任务

本文介绍如何将数据从消息队列Kafka版实例的数据源Topic导出至对象存储。

## 前提条件

在导出数据前，请确保您已完成以下操作：

-   为消息队列Kafka版实例开启Connector。更多信息，请参见[开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)。
-   为消息队列Kafka版实例创建数据源Topic。更多信息，请参见[步骤一：创建Topic](/cn.zh-CN/快速入门/步骤三：创建资源.md)。
-   在[OSS管理控制台](https://oss.console.aliyun.com/bucket)创建存储空间。更多信息，请参见[创建存储空间](https://help.aliyun.com/document_detail/31885.html?spm=a2c4g.11174283.6.614.1bf37da24aeBfe)。
-   开通函数计算服务。更多信息，请参见[开通函数计算服务]()。

## 注意事项

-   仅支持在同地域内，将数据从消息队列Kafka版实例的数据源Topic导出至函数计算，再由函数计算导出至对象存储。Connector的限制说明，请参见[使用限制](/cn.zh-CN/用户指南/Connector/Connector概述.md)。
-   该功能基于函数计算服务提供。函数计算为您提供了一定的免费额度，超额部分将产生费用，请以函数计算的计费规则为准。计费详情，请参见[计费概述]()。
-   函数计算的函数调用支持日志查询。具体操作步骤，请参见[配置并查看函数日志]()。
-   消息转储时，消息队列Kafka版中消息用UTF-8 String序列化，暂不支持二进制的数据格式。

## 创建并部署对象存储Connector

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Connector（公测组件）**。

6.  在**Connector（公测组件）**页面，单击**创建Connector**。

7.  在**创建Connector**配置向导中，完成以下操作。

    1.  在**基础信息**下方的**Connector名称**文本框，输入Connector名称，从**转储路径**列表，选择**消息队列Kafka版**，从**转储到**列表，选择**对象存储**，单击**下一步**。

        **说明：** 消息队列Kafka版会为您自动选中**授权创建服务关联角色**。

        -   如果未创建服务关联角色，消息队列Kafka版会为您自动创建一个服务关联角色，以便您使用消息队列Kafka版导出数据至对象存储的功能。
        -   如果已创建服务关联角色，消息队列Kafka版不会重复创建。
        关于该服务关联角色的更多信息，请参见[服务关联角色](/cn.zh-CN/权限控制/服务关联角色.md)。

        |参数|描述|示例值|
        |--|--|---|
        |Connector名称|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-oss-sink|
        |任务类型|Connector的数据同步任务类型。本文以数据从消息队列Kafka版导出至对象存储为例。更多任务类型，请参见[Connector类型](/cn.zh-CN/用户指南/Connector/Connector概述.md)。|KAFKA2OSS|

    2.  在**源实例配置**下方的**数据源Topic**列表，选择数据源Topic的名称，从**消费初始位置**列表，选择消费初始位置，从**消费线程并发数**列表，选择消费线程并发数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |数据源Topic|需要同步数据的Topic。|oss-test-input|
        |消费初始位置|开始消费的位置。取值：         -   latest：从最新位点开始消费。
        -   earliest：从最初位点开始消费。
|latest|
        |消费线程并发数|数据源Topic的消费线程并发数。默认值为6。取值：        -   6
        -   12
|6|

    3.  在**目标实例配置 运行环境配置**下方，配置目标实例相关参数。

        |参数|描述|示例值|
        |--|--|---|
        |Bucket名称|对象存储Bucket的名称。|bucket\_test|
        |AccessKey ID|阿里云账号的AccessKey ID。|LTAI4GG2RGAjppjK\*\*\*\*\*\*\*\*|
        |AccessKey Secret|阿里云账号的AccessKey Secret。|WbGPVb5rrecVw3SQvEPw6R\*\*\*\*\*\*\*\*|

        请确保您使用的AccessKey ID所对应的账号已被授予以下最小权限：

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "oss:GetObject",
                        "oss:PutObject"
                    ],
                    "Resource": "*",
                    "Effect": "Allow"
                }
            ]
        }
        ```

        **说明：**

        AccessKey ID和AccessKey Secret是消息队列Kafka版创建任务时作为环境变量传递至函数计算的函数，任务创建成功后，消息队列Kafka版不保存AccessKey ID和AccessKey Secret信息。

    4.  在**目标实例配置 运行环境配置**下方，配置运行环境，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |VPC ID|数据同步任务所在的VPC。默认为消息队列Kafka版实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|数据同步任务所在的交换机。该交换机必须与消息队列Kafka版实例处于同一VPC。默认为部署消息队列Kafka版实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |失败处理策略|消息发送失败后的错误处理。默认为log。取值：        -   log：继续订阅出现错误的Topic的分区，并打印错误日志。出现错误后，您可以通过Connector日志查看错误，并根据错误码查找解决方案，以进行自助排查。

**说明：**

            -   如何查看Connector日志，请参见[查看Connector日志](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)。
            -   如何根据错误码查找解决方案，请参见[错误码]()。
        -   fail：停止对出现错误的Topic的分区的订阅，并打印错误日志。出现错误后，您可以通过Connector日志查看错误，并根据错误的错误码查找解决方案，以进行自助排查。

**说明：**

            -   如何查看Connector日志，请参见[查看Connector日志](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)。
            -   如何根据错误码查找解决方案，请参见[错误码]()。
            -   如需恢复对出现错误的Topic的分区的订阅，您需要提交[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=a2c4g.11186623.2.23.33183cc5K5SAef)联系消息队列Kafka版技术人员。
|log|
        |创建资源|选择**自动创建**或者选择**手动创建**并输入手动创建的Topic的名称。|自动创建|
        |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|connect-cluster-kafka-oss-sink|
        |任务位点Topic|用于存储消费位点的Topic。        -   Topic名称：建议以connect-offset开头。
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-oss-sink|
        |任务配置Topic|用于存储任务配置的Topic。        -   Topic名称：建议以connect-config开头。
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-oss-sink|
        |任务状态Topic|用于存储任务状态的Topic。        -   Topic名称：建议以connect-status开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-oss-sink|
        |异常数据Topic|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-oss-sink|
        |死信队列Topic|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-oss-sink|

    5.  在**预览/提交**下方，确认Connector的配置，然后单击**提交**。

8.  在**创建Connector**面板，单击**部署**。


## 发送测试消息

您可以向消息队列Kafka版的数据源Topic发送消息，测试数据能否被导出至对象存储。

1.  在**Connector（公测组件）**页面，找到目标Connector，在其右侧**操作**列，单击**测试**。

2.  在**Topic管理**页面，选择实例，找到数据源Topic，在其右侧**操作**列，单击**发送消息**。

3.  在**发送消息**面板，发送测试消息。

    1.  在**分区**文本框，输入0。

    2.  在**Message Key**文本框，输入1。

    3.  在**Message Value**文本框，输入1。

    4.  单击**发送**。


## 验证结果

向消息队列Kafka版的数据源Topic发送消息后，查看OSS文件管理，验证数据导出结果。更多信息，请参见[文件概览](https://help.aliyun.com/document_detail/31908.html?spm=a2c4g.11186623.6.1828.183d62e7KAJ360)。

文件管理中显示新导出的文件。

![files](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7263324161/p243372.png)

消息队列Kafka版数据导出至对象存储的格式示例如下：

```
[
    {
        "key":"123",
        "offset":4,
        "overflowFlag":true,
        "partition":0,
        "timestamp":1603779578478,
        "topic":"Test",
        "value":"1",
        "valueSize":272687
    }
]
```

## 更多操作

您可以按需对该Connector所依赖的函数计算资源进行配置。

1.  在**Connector（公测组件）**页面，找到目标Connector，在其右侧**操作**列，单击**函数配置**。

    页面跳转至函数计算控制台，您可以按需配置函数资源。


