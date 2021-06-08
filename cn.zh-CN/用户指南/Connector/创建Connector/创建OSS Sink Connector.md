# 创建OSS Sink Connector

本文介绍如何创建OSS Sink Connector将数据从实例的数据源Topic导出至对象存储OSS。

## 前提条件

在导出数据前，请确保您已完成以下操作：

-   为实例开启Connector。更多信息，请参见[t1908611.md\#](/cn.zh-CN/用户指南/Connector/开启Connector.md)。
-   为实例创建数据源Topic。更多信息，请参见[t998824.md\#section\_zm0\_ysj\_343](/cn.zh-CN/快速入门/步骤三：创建资源.md)。
-   在[OSS管理控制台](https://oss.console.aliyun.com/bucket)创建存储空间。更多信息，请参见[t4333.md\#](/cn.zh-CN/快速入门/控制台快速入门/创建存储空间.md)。
-   开通函数计算服务。更多信息，请参见[开通函数计算服务]()。

## 注意事项

-   仅支持在同地域内，将数据从实例的数据源Topic导出至函数计算，再由函数计算导出至对象存储。Connector的限制说明，请参见[t1908590.md\#section\_cgo\_h01\_5e3](/cn.zh-CN/用户指南/Connector/Connector概述.md)。
-   该功能基于函数计算服务提供。函数计算为您提供了一定的免费额度，超额部分将产生费用，请以函数计算的计费规则为准。计费详情，请参见[t1880954.md\#]()。
-   函数计算的函数调用支持日志查询。具体操作步骤，请参见[t1881019.md\#]()。
-   消息转储时，中消息用UTF-8 String序列化，暂不支持二进制的数据格式。

## 创建并部署OSS Sink Connector

1.  在**创建 Connector**配置向导面页面，完成以下操作。

    1.  在**配置基本信息**页签，按需配置以下参数，然后单击**下一步**。

        **说明：** 会为您自动选中**授权创建服务关联角色**。

        -   如果未创建服务关联角色，会为您自动创建一个服务关联角色，以便您使用导出数据至对象存储的功能。
        -   如果已创建服务关联角色，不会重复创建。
        关于该服务关联角色的更多信息，请参见[t1998690.md\#](/cn.zh-CN/权限控制/服务关联角色.md)。

        |参数|描述|示例值|
        |--|--|---|
        |**名称**|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-oss-sink|
        |**实例**|默认配置为实例的名称与实例ID。|demo alikafka\_post-cn-st21p8vj\*\*\*\*|

    2.  在**配置源服务**页签，选择数据源为消息队列Kafka版，并配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**数据源 Topic**|需要同步数据的Topic。|oss-test-input|
        |**消费线程并发数**|数据源Topic的消费线程并发数。默认值为6。取值说明如下：        -   **6**
        -   **12**
|**6**|
        |**消费初始位置**|开始消费的位置。取值说明如下：         -   **最早位点**：从最初位点开始消费。
        -   **最近位点**：从最新位点开始消费。
|**最早位点**|
        |**VPC ID**|数据同步任务所在的VPC。单击**配置运行环境**显示该参数。默认为实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch ID**|数据同步任务所在的交换机。单击**配置运行环境**显示该参数。该交换机必须与实例处于同一VPC。默认为部署实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |**失败处理**|消息发送失败后，是否继续订阅出现错误的Topic的分区。单击**配置运行环境**显示该参数。取值说明如下。        -   **继续订阅**：继续订阅出现错误的Topic的分区，并打印错误日志。
        -   **停止订阅**：停止订阅出现错误的Topic的分区，并打印错误日志
**说明：**

        -   如何查看日志，请参见[t1909714.md\#](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)。
        -   如何根据错误码查找解决方案，请参见[t1881176.md\#section\_o0o\_6fa\_tcl]()。
        -   如需恢复对出现错误的Topic的分区的订阅，您需要[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=a2c4g.11186623.2.23.33183cc5K5SAef)联系技术人员。
|**继续订阅**|
        |**创建资源方式**|选择创建Connector所依赖的Topic与Group的方式。单击**配置运行环境**显示该参数。        -   **自动创建**
        -   **手动创建**
|**自动创建**|
        |**Connector 消费组**|Connector使用的Consumer Group。单击**配置运行环境**显示该参数。该Consumer Group的名称建议以connect-cluster开头。|connect-cluster-kafka-oss-sink|
        |**任务位点 Topic**|用于存储消费位点的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-offset开头。
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-oss-sink|
        |**任务配置 Topic**|用于存储任务配置的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-config开头。
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-oss-sink|
        |**任务状态 Topic**|用于存储任务状态的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-status开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-oss-sink|
        |**死信队列 Topic**|用于存储Connect框架的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-oss-sink|
        |**异常数据 Topic**|用于存储Sink的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-oss-sink|

    3.  在**配置目标服务**页签，选择目标服务为对象存储，并配置以下参数，然后单击**创建**。

        |参数|描述|示例值|
        |--|--|---|
        |**Bucket 名称**|对象存储Bucket的名称。|bucket\_test|
        |**Access Key**|阿里云账号的AccessKey ID。|LTAI4GG2RGAjppjK\*\*\*\*\*\*\*\*|
        |**Secret Key**|阿里云账号的AccessKey Secret。|WbGPVb5rrecVw3SQvEPw6R\*\*\*\*\*\*\*\*|

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

        AccessKey ID和AccessKey Secret是创建任务时作为环境变量传递至对象存储的数据，任务创建成功后，不保存AccessKey ID和AccessKey Secret信息。

        创建完成后，在**Connector 管理**页面，查看创建的Connector 。

2.  创建完成后，在**Connector 管理**页面，找到创建的Connector ，单击其**操作**列的**部署**。


## 发送测试消息

您可以向的数据源Topic发送消息，测试数据能否被导出至对象存储。


## 验证结果

向的数据源Topic发送消息后，查看OSS文件管理，验证数据导出结果。更多信息，请参见[t4752.md\#](/cn.zh-CN/控制台用户指南/文件管理/文件概览.md)。

文件管理中显示新导出的文件。

![files](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7263324161/p243372.png)

数据导出至对象存储的格式示例如下：

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

1.  在**Connector 管理**页面，找到创建的Connector，单击其**操作**列的**更多** \> **配置函数**。

    页面跳转至函数计算控制台，您可以按需配置函数资源。


