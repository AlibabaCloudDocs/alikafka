---
keyword: [kafka, connector, fc]
---

# 创建FC Sink Connector

本文说明如何创建FC Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至函数计算的函数。

在创建FC Sink Connector前，请确保您已完成以下操作：

-   为消息队列Kafka版实例开启Connector。更多信息，请参见[开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)。
-   为消息队列Kafka版实例创建数据源Topic。更多信息，请参见[步骤一：创建Topic](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

    本文以名称为fc-test-input的Topic为例。

-   在函数计算创建函数。更多信息，请参见[使用控制台创建函数]()。

    **说明：** 函数类型必须为事件函数。

    本文以服务名称为guide-hello\_world、函数名称为hello\_world、运行环境为Python的事件函数为例。该示例函数的代码如下：

    ```
    # -*- coding: utf-8 -*-
    import logging
    
    # To enable the initializer feature
    # please implement the initializer function as below:
    # def initializer(context):
    #   logger = logging.getLogger()
    #   logger.info('initializing')
    
    def handler(event, context):
      logger = logging.getLogger()
      logger.info('hello world:' + bytes.decode(event))
      return 'hello world:' + bytes.decode(event)
    ```


## 操作流程

使用FC Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至函数计算的函数的操作流程如下：

1.  使FC Sink Connector跨地域访问函数计算

    **说明：** 如果您不需要使FC Sink Connector跨地域访问函数计算，您可以直接跳过该步骤。

    [为FC Sink Connector开启公网访问](#section_y3y_7cd_gpk)

2.  使FC Sink Connector跨账号访问函数计算

    **说明：** 如果您不需要使FC Sink Connector跨账号访问函数计算，您可以直接跳过该步骤。

    -   [创建自定义权限策略](#section_3wj_qkk_gwt)
    -   [创建RAM角色](#section_24p_yc7_s0d)
    -   [添加权限](#section_co0_y32_ams)
3.  创建FC Sink Connector依赖的Topic和Consumer Group

    **说明：**

    -   如果您不需要自定义Topic和Consumer Group的名称，您可以直接跳过该步骤。
    -   部分FC Sink Connector依赖的Topic的存储引擎必须为Local存储，大版本为0.10.2的消息队列Kafka版实例不支持手动创建Local存储的Topic，只支持自动创建。
    1.  [创建FC Sink Connector依赖的Topic](#section_0wn_cbs_hf5)
    2.  [创建FC Sink Connector依赖的Consumer Group](#section_fbf_mav_odr)
4.  [创建并部署FC Sink Connector](#section_4dk_lib_xrh)
5.  结果验证
    1.  [发送测试消息](#section_rt2_26k_a0s)
    2.  [查看函数日志](#section_off_gyb_3lk)

## 为FC Sink Connector开启公网访问

如需使FC Sink Connector跨地域访问其他阿里云服务，您需要为FC Sink Connector开启公网访问。具体操作，请参见[为Connector开启公网访问](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。

## 创建自定义权限策略

在目标账号下创建访问函数计算的自定义权限策略。

1.  登录[访问控制控制台](https://ram.console.aliyun.com/)。

2.  在左侧导航栏，选择**权限管理** \> **权限策略管理**。

3.  在**权限策略管理**页面，单击**创建权限策略**。

4.  在**新建自定义权限策略**页面，创建自定义权限策略。

    1.  在**策略名称**文本框，输入KafkaConnectorFcAccess。

    2.  在**配置模式**区域，选择**脚本配置**。

    3.  在**策略内容**区域，输入自定义权限策略脚本。

        访问函数计算的自定义权限策略脚本示例如下：

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "fc:InvokeFunction",
                        "fc:GetFunction"
                    ],
                    "Resource": "*",
                    "Effect": "Allow"
                }
            ]
        }
        ```

    4.  单击**确定**。


## 创建RAM角色

在目标账号下创建RAM角色。由于RAM角色不支持直接选择消息队列Kafka版作为受信服务，您在创建RAM角色时，需要选择任意支持的服务作为受信服务。RAM角色创建后，手工修改信任策略。

1.  在左侧导航栏，单击**RAM角色管理**。

2.  在**RAM角色管理**页面，单击**创建RAM角色**。

3.  在**创建RAM角色**面板，创建RAM角色。

    1.  在**当前可信实体类型**区域，选择**阿里云服务**，单击**下一步**。

    2.  在**角色类型**区域，选择**普通服务角色**，在**角色名称**文本框，输入AliyunKafkaConnectorRole，从**选择受信服务**列表，选择**函数计算**，然后单击**完成**。

4.  在**RAM角色管理**页面，找到**AliyunKafkaConnectorRole**，单击**AliyunKafkaConnectorRole**。

5.  在**AliyunKafkaConnectorRole**页面，单击**信任策略管理**页签，单击**修改信任策略**。

6.  在**修改信任策略**面板，将脚本中**fc**替换为alikafka，单击**确定**。

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3906119951/p128120.png)


## 添加权限

在目标账号下为创建的RAM角色授予访问函数计算的权限。

1.  在左侧导航栏，单击**RAM角色管理**。

2.  在**RAM角色管理**页面，找到**AliyunKafkaConnectorRole**，在其右侧**操作**列，单击**添加权限**。

3.  在**添加权限**面板，添加**KafkaConnectorFcAccess**权限。

    1.  在**选择权限**区域，选择**自定义策略**。

    2.  在**权限策略名称**列表，找到**KafkaConnectorFcAccess**，单击**KafkaConnectorFcAccess**。

    3.  单击**确定**。

    4.  单击**完成**。


## 创建FC Sink Connector依赖的Topic

您可以在消息队列Kafka版控制台手动创建FC Sink Connector依赖的5个Topic。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Topic管理**。

6.  在**Topic管理**页面，单击**创建 Topic**。

7.  在**创建 Topic**面板，设置Topic属性，然后单击**创建**。

    |Topic|描述|
    |-----|--|
    |任务位点Topic|用于存储消费位点的Topic。    -   Topic：建议以connect-offset开头。
    -   分区数：Topic的分区数量必须大于1。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |任务配置Topic|用于存储任务配置的Topic。    -   Topic：建议以connect-config开头。
    -   分区数：Topic的分区数量必须为1。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |任务状态Topic|用于存储任务状态的Topic。    -   Topic：建议以connect-status开头。
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |死信队列Topic|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。    -   Topic：建议以connect-error开头。
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎可以为Local存储或云存储。 |
    |异常数据Topic|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。    -   Topic：建议以connect-error开头。
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎可以为Local存储或云存储。 |


## 创建FC Sink Connector依赖的Consumer Group

您可以在消息队列Kafka版控制台手动创建FC Sink Connector依赖的2个Consumer Group。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Consumer Group管理**。

6.  在**Consumer Group管理**页面，单击**创建Consumer Group**。

7.  在**创建Consumer Group**面板，设置Consumer Group属性，然后单击**创建**。

    |Consumer Group|描述|
    |--------------|--|
    |Connector任务消费组|Connector的数据同步任务使用的Consumer Group。该Consumer Group的名称必须为connect-任务名称。|
    |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|


## 创建并部署FC Sink Connector

创建并部署用于将数据从消息队列Kafka版同步至函数计算的FC Sink Connector。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Connector（公测组件）**。

6.  在**Connector（公测组件）**页面，单击**创建Connector**。

7.  在**创建Connector**面板，完成以下操作。

    1.  在**基础信息**页签，按需配置以下参数，然后单击**下一步**。

        **说明：** 消息队列Kafka版会为您自动选中**授权创建服务关联角色**。

        -   如果未创建服务关联角色，消息队列Kafka版会为您自动创建一个服务关联角色，以便您使用FC Sink Connector。
        -   如果已创建服务关联角色，消息队列Kafka版不会重复创建。
        服务关联角色的更多信息，请参见[服务关联角色](/cn.zh-CN/权限控制/服务关联角色.md)。

        |参数|描述|示例值|
        |--|--|---|
        |**Connector名称**|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-fc-sink|
        |**转储路径**|配置数据转储的源和目标。第一个下拉列表中选择数据源，第二个下拉列表中选择目标。|从**消息队列Kafka版**转储到**函数计算**|

    2.  在**源实例配置**页签，配置以下参数，然后单击**下一步**。

        **说明：** 如果您已创建好Topic和Consumer Group，那么请选择手动创建资源，并填写已创建的资源信息。否则，请选择自动创建资源。

        |参数|描述|示例值|
        |--|--|---|
        |**VPC ID**|数据同步任务所在的VPC。默认为消息队列Kafka版实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch ID**|数据同步任务所在的交换机。该交换机必须与消息队列Kafka版实例处于同一VPC。默认为部署消息队列Kafka版实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |**数据源Topic**|需要同步数据的Topic。|fc-test-input|
        |**消费初始位置**|开始消费的位置。取值说明如下：         -   **latest**：从最新位点开始消费。
        -   **earliest**：从最初位点开始消费。
|latest|
        |**消费线程并发数**|数据源Topic的消费线程并发数。默认值为3。取值说明如下：        -   **3**
        -   **6**
        -   **9**
        -   **12**
|3|
        |**Connector消费组**|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|connect-cluster-kafka-fc-sink|
        |**任务位点Topic**|用于存储消费位点的Topic。        -   Topic：建议以connect-offset开头。
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-fc-sink|
        |**任务配置Topic**|用于存储任务配置的Topic。        -   Topic：建议以connect-config开头。
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-fc-sink|
        |**任务状态Topic**|用于存储任务状态的Topic。        -   Topic：建议以connect-status开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-fc-sink|
        |**死信队列Topic**|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-fc-sink|
        |**异常数据Topic**|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-fc-sink|

    3.  在**目标实例配置**页签，配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**是否跨账号/地域**|FC Sink Connector是否跨账号/地域向函数计算服务同步数据。默认为**否**。取值：        -   **否**：同地域同账号模式。
        -   **是**：跨地域同账号模式、同地域跨账号模式或跨地域跨账号模式。
|否|
        |**服务地域**|函数计算服务的地域。默认为FC Sink Connector所在地域。如需跨地域，您需要为Connector开启公网访问，然后选择目标地域。更多信息，请参见[为FC Sink Connector开启公网访问](#section_y3y_7cd_gpk)。**说明：** **是否跨账号/地域**为**是**时，显示**服务地域**。

|cn-hangzhou|
        |**服务接入点**|函数计算服务的接入点。在[函数计算控制台](https://fc.console.aliyun.com/fc/overview/cn-hangzhou)的**概览**页的**常用信息**区域获取。         -   内网Endpoint：低延迟，推荐。适用于消息队列Kafka版实例和函数计算处于同一地域场景。
        -   公网Endpoint：高延迟，不推荐。适用于消息队列Kafka版实例和函数计算处于不同地域的场景。如需使用公网Endpoint，您需要为Connector开启公网访问。更多信息，请参见[为FC Sink Connector开启公网访问](#section_y3y_7cd_gpk)。
**说明：** **是否跨账号/地域**为**是**时，显示**服务接入点**。

|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
        |**服务账号**|函数计算服务的阿里云账号ID。在函数计算控制台的**概览**页的**常用信息**区域获取。**说明：** **是否跨账号/地域**为**是**时，显示**服务账号**。

|188\*\*\*|
        |**授权角色名**|消息队列Kafka版访问函数计算服务的RAM角色。        -   如不需跨账号，您需要在本账号下创建RAM角色并为其授权，然后输入该授权角色名。操作步骤，请参见[创建自定义权限策略](#section_3wj_qkk_gwt)、[创建RAM角色](#section_24p_yc7_s0d)和[添加权限](#section_co0_y32_ams)。
        -   如需跨账号，您需要在目标账号下创建RAM角色并为其授权，然后输入该授权角色名。操作步骤，请参见[创建自定义权限策略](#section_3wj_qkk_gwt)、[创建RAM角色](#section_24p_yc7_s0d)和[添加权限](#section_co0_y32_ams)。
**说明：** **是否跨账号/地域**为**是**时，显示**授权角色名**。

|AliyunKafkaConnectorRole|
        |**服务名**|函数计算服务的名称。|guide-hello\_world|
        |**函数名**|函数计算服务的函数名称。|hello\_world|
        |**服务版本或别名**|函数计算服务的版本或别名。|LATEST|
        |**发送模式**|消息发送模式。取值说明如下：         -   **异步**：推荐。
        -   **同步**：不推荐。同步发送模式下，如果函数计算的处理时间较长，消息队列Kafka版的处理时间也会较长。当同一批次消息的处理时间超过5分钟时，会触发消息队列Kafka版客户端Rebalance。
|异步|
        |**发送批大小**|批量发送的消息条数。默认为20。Connector根据发送批次大小和请求大小限制（同步请求大小限制为6 MB，异步请求大小限制为128 KB）将多条消息聚合后发送。例如，发送模式为异步，发送批次大小为20，如果要发送18条消息，其中有17条消息的总大小为127 KB，有1条消息的大小为200 KB，Connector会将总大小不超过128 KB的17条消息聚合后发送，将大小超过128 KB的1条消息单独发送。**说明：** 如果您在发送消息时将key设置为null，则请求中不包含key。如果将value设置为null，则请求中不包含value。

        -   如果批量发送的多条消息的大小不超过请求大小限制，则请求中包含消息内容。请求示例如下：

            ```
[
    {
        "key":"this is the message's key2",
        "offset":8,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325438,
        "topic":"Test",
        "value":"this is the message's value2",
        "valueSize":28
    },
    {
        "key":"this is the message's key9",
        "offset":9,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325440,
        "topic":"Test",
        "value":"this is the message's value9",
        "valueSize":28
    },
    {
        "key":"this is the message's key12",
        "offset":10,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325442,
        "topic":"Test",
        "value":"this is the message's value12",
        "valueSize":29
    },
    {
        "key":"this is the message's key38",
        "offset":11,
        "overflowFlag":false,
        "partition":4,
        "timestamp":1603785325464,
        "topic":"Test",
        "value":"this is the message's value38",
        "valueSize":29
    }
]
            ```

        -   如果发送的单条消息的大小超过请求大小限制，则请求中不包含消息内容。请求示例如下：

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

**说明：** 如需获取消息内容，您需要根据位点主动拉取消息。

|50|
        |**重试次数**|消息发送失败后的重试次数。默认为2。取值范围为1~3。部分导致消息发送失败的错误不支持重试。[错误码]()与是否支持重试的对应关系如下：        -   4XX：除429支持重试外，其余错误码不支持重试。
        -   5XX：支持重试。
**说明：** Connector调用[InvokeFunction]()向函数计算发送消息。

|2|
        |**失败处理**|消息发送失败后，是否继续订阅出现错误的Topic的分区。取值说明如下：        -   log：继续订阅出现错误的Topic的分区，并打印错误日志。
        -   fail：停止订阅出现错误的Topic的分区，并打印错误日志。
**说明：**

        -   如何查看日志，请参见[查看Connector日志](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)。
        -   如何根据错误码查找解决方案，请参见[错误码]()。
        -   如需恢复对出现错误的Topic的分区的订阅，您需要[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=a2c4g.11186623.2.23.33183cc5K5SAef)联系消息队列Kafka版技术人员。
|log|

    4.  在**预览/提交**页签，确认Connector的配置，然后单击**提交**。

8.  在**创建Connector**面板，单击**部署**。

    如需配置函数计算资源，单击**函数配置**，跳转至函数计算控制台完成操作。


## 发送测试消息

部署FC Sink Connector后，您可以向消息队列Kafka版的数据源Topic发送消息，测试数据能否被同步至函数计算。

1.  在**Connector（公测组件）**页面，找到目标Connector，在其右侧**操作**列，单击**测试**。

2.  在**Topic管理**页面，选择实例，找到**fc-test-input**，在其右侧**操作**列，单击**发送消息**。

3.  在**发送消息**面板，发送测试消息。

    1.  在**分区**文本框，输入0。

    2.  在**Message Key**文本框，输入1。

    3.  在**Message Value**文本框，输入1。

    4.  单击**发送**。


## 查看函数日志

向消息队列Kafka版的数据源Topic发送消息后，查看函数日志，验证是否收到消息。更多信息，请参见[配置并查看函数日志]()。

日志中显示发送的测试消息。

![fc LOG](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7259955061/p127831.png)

