---
keyword: [kafka, connector, fc]
---

# 创建FC Sink Connector

本文说明如何创建FC Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至函数计算的函数。

在创建FC Sink Connector前，请确保您已完成以下操作：

1.  为消息队列Kafka版实例开启Connector。详情请参见[开启Connector](/intl.zh-CN/用户指南/Connector/开启Connector.md)。
2.  为消息队列Kafka版实例创建数据源Topic。详情请参见[步骤一：创建Topic](/intl.zh-CN/快速入门/步骤三：创建资源.md)。

    本文以名称为fc-test-input的Topic为例。

3.  在函数计算创建函数。详情请参见[使用控制台创建函数]()。

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

1.  授予消息队列Kafka版访问函数计算的权限
    1.  [创建自定义权限策略](#section_ein_kur_ed3)
    2.  [创建RAM角色](#section_onx_vzm_c6q)
    3.  [添加权限](#section_ubx_dy2_thy)
2.  创建FC Sink Connector依赖的Topic和Consumer Group（可选）

    如果您不需要自定义Topic和Consumer Group的名称，您可以直接跳过该步骤，在下一步骤选择自动创建。

    **说明：** 部分FC Sink Connector依赖的Topic的存储引擎必须为Local存储，大版本为0.10.2的消息队列Kafka版实例不支持手动创建Local存储的Topic，只支持自动创建。

    1.  [创建FC Sink Connector依赖的Topic（可选）](#section_0wn_cbs_hf5)
    2.  [创建FC Sink Connector依赖的Consumer Group（可选）](#section_fbf_mav_odr)
3.  创建并部署FC Sink Connector
    1.  [创建FC Sink Connector](#section_4dk_lib_xrh)
    2.  [部署FC Sink Connector](#section_h1f_aa2_ydg)
4.  结果验证
    1.  [发送消息](#section_rt2_26k_a0s)
    2.  [查看函数日志](#section_off_gyb_3lk)

## 创建自定义权限策略

创建访问函数计算的自定义权限策略。

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

由于RAM角色不支持直接选择消息队列Kafka版作为受信服务，您在创建RAM角色时，需要选择任意支持的服务作为受信服务。RAM角色创建后，手工修改信任策略。

1.  在左侧导航栏，单击**RAM角色管理**。

2.  在**RAM角色管理**页面，单击**创建RAM角色**。

3.  在**创建RAM角色**面板，创建RAM角色。

    1.  在**当前可信实体类型**区域，选择**阿里云服务**，单击**下一步**。

    2.  在**角色类型**区域，选择**普通服务角色**，在**角色名称**文本框，输入AliyunKafkaConnectorRole，从**选择受信服务**列表，选择**函数计算**，然后单击**完成**。

4.  在**RAM角色管理**页面，找到**AliyunKafkaConnectorRole**，单击**AliyunKafkaConnectorRole**。

5.  在**AliyunKafkaConnectorRole**页面，单击**信任策略管理**页签，单击**修改信任策略**。

6.  在**修改信任策略**对话框，将脚本中**fc**替换为alikafka，单击**确定**。

    ![AliyunKafkaConnectorRole](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3906119951/p128120.png)


## 添加权限

为创建的RAM角色授予访问函数计算的权限。

1.  在左侧导航栏，单击**RAM角色管理**。

2.  在**RAM角色管理**页面，找到**AliyunKafkaConnectorRole**，在其右侧**操作**列，单击**添加权限**。

3.  在**添加权限**对话框，添加**KafkaConnectorFcAccess**权限。

    1.  在**选择权限**面板，选择**自定义权限策略**。

    2.  在**权限策略名称**列表，找到**KafkaConnectorFcAccess**，单击**KafkaConnectorFcAccess**。

    3.  单击**确定**。

    4.  单击**完成**。


## 创建FC Sink Connector依赖的Topic（可选）

您可以在消息队列Kafka版控制台手动创建FC Sink Connector依赖的5个Topic。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**Topic管理**。

4.  在**Topic管理**页面，选择实例，单击**创建Topic**。

5.  在**创建Topic**对话框，设置Topic属性，然后单击**创建**。

    |Topic|描述|
    |-----|--|
    |任务位点Topic|用于存储消费位点的Topic。    -   Topic名称：建议以connect-offset开头
    -   分区数：Topic的分区数量必须大于1。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |任务配置Topic|用于存储任务配置的Topic。    -   Topic名称：建议以connect-config开头
    -   分区数：Topic的分区数量必须为1。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |任务状态Topic|用于存储任务状态的Topic。    -   Topic名称：建议以connect-status开头
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎必须为Local存储。
    -   cleanup.policy：Topic的日志清理策略必须为compact。 |
    |死信队列Topic|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。    -   Topic名称：建议以connect-error开头
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎可以为Local存储或云存储。 |
    |异常数据Topic|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。    -   Topic名称：建议以connect-error开头
    -   分区数：Topic的分区数量建议为6。
    -   存储引擎：Topic的存储引擎可以为Local存储或云存储。 |


## 创建FC Sink Connector依赖的Consumer Group（可选）

您可以在消息队列Kafka版控制台手动创建FC Sink Connector依赖的2个Consumer Group。

1.  在左侧导航栏，单击**Consumer Group管理**。

2.  在**Consumer Group管理**页面，选择实例，单击**创建Consumer Group**。

3.  在**创建Consumer Group**对话框，设置Topic属性，然后单击**创建**。

    |Consumer Group|描述|
    |--------------|--|
    |Connector任务消费组|Connector的数据同步任务使用的Consumer Group。该Consumer Group的名称必须为connect-任务名称。|
    |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|


## 创建FC Sink Connector

授予消息队列Kafka版访问函数计算的权限后，创建用于将数据从消息队列Kafka版同步至函数计算的FC Sink Connector。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**Connector**。

4.  在**Connector**页面，选择实例，单击**创建Connector**。

5.  在**创建Connector**面板，完成以下操作。

    1.  在**基础信息**下方的**Connector名称**文本框，输入Connector名称，从**转储路径**列表，选择**消息队列Kafka版**，从**转储到**列表，选择**函数计算**，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |Connector名称|Connector的名称。取值：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-fc-sink|
        |任务类型|Connector的数据同步任务类型。本文以数据从消息队列Kafka版同步至函数计算为例。更多任务类型，请参见[Connector类型](/intl.zh-CN/用户指南/Connector/Connector概述.md)。|KAFKA2FC|

    2.  在**源实例配置**下方的**数据源Topic**文本框，输入数据源Topic的名称，从**消费初始位置**列表，选择消费初始位置，在**创建资源**，选择**自动创建**或者选择**手动创建**并输入手动创建的Topic的名称，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |VPC ID|数据同步任务所在的VPC。默认为消息队列Kafka版实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|数据同步任务所在的交换机。用户交换机必须与消息队列Kafka版实例处于同一VPC。默认为部署消息队列Kafka版实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |数据源Topic|需要同步数据的Topic。|fc-test-input|
        |消费初始位置|开始消费的位置。取值：         -   latest：从最新位点开始消费。
        -   earliest：从最初位点开始消费。
|latest|
        |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|connect-cluster-kafka-fc-sink|
        |任务位点Topic|用于存储消费位点的Topic。        -   Topic名称：建议以connect-offset开头
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-fc-sink|
        |任务配置Topic|用于存储任务配置的Topic。        -   Topic名称：建议以connect-config开头
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-fc-sink|
        |任务状态Topic|用于存储任务状态的Topic。        -   Topic名称：建议以connect-status开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-fc-sink|
        |死信队列Topic|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-fc-sink|
        |异常数据Topic|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-fc-sink|

    3.  在**目标实例配置**下方，输入函数计算服务的属性，设置发送模式和发送批大小，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |服务地域|函数计算服务的地域。|cn-hangzhou|
        |服务接入点|函数计算服务的接入点。在函数计算控制台的**概览**页的**常用信息**区域获取。         -   内网Endpoint：低延迟，推荐。适用于消息队列Kafka版实例和函数计算处于同一地域场景。
        -   公网Endpoint：高延迟，不推荐。适用于消息队列Kafka版实例和函数计算处于不同地域的场景。如需使用公网Endpoint，您需要为Connector开启公网访问。详情请参见[为Connector开启公网访问](/intl.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。
|http://188\*\*\*.cn-hangzhou.fc.aliyuncs.com|
        |服务账号|函数计算服务的阿里云账号ID。在函数计算控制台的**概览**页的**常用信息**区域获取。|188\*\*\*|
        |授权角色名|消息队列Kafka版的RAM角色的名称。详情请参见[创建RAM角色](#section_onx_vzm_c6q)。|AliyunKafkaConnectorRole|
        |服务名|函数计算服务的名称。|guide-hello\_world|
        |服务方法名|函数计算服务的函数名称。|hello\_world|
        |服务版本|函数计算服务的版本。|LATEST|
        |发送模式|数据发送模式。取值：         -   异步：推荐。
        -   同步：不推荐。同步发送模式下，如果函数计算的处理时间较长，消息队列Kafka版的处理时间也会较长。当同一批次数据的处理时间超过5分钟时，会触发消息队列Kafka版客户端Rebalance。
|异步|
        |发送批次大小|批量发送消息的大小。默认为20。同步任务会根据该值以及同步或异步请求的大小限制（同步请求大小限制为6 MB，异步请求大小限制为128 KB）将数据合并发送。如果单条数据大小超过请求大小上限，数据内容将不会包含在请求中，您可以通过offset主动拉取消息队列Kafka版数据。|50|

6.  在**预览/创建**下方，确认Connector的配置，然后单击**提交**。

    提交完成后，刷新**Connector**页面以显示创建的Connector。


## 部署FC Sink Connector

创建FC Sink Connector后，您需要部署该FC Sink Connector以使其将数据从消息队列Kafka版同步至函数计算。

1.  在**Connector**页面，找到创建的FC Sink Connector，在其右侧**操作**列，单击**部署**。

    部署完成后，Connector状态显示运行中。


## 发送消息

部署FC Sink Connector后，您可以向消息队列Kafka版的数据源Topic发送消息，测试数据能否被同步至函数计算。

1.  在左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，选择实例，找到**fc-test-input**，在其右侧**操作**列，单击**发送消息**。

3.  在**发送消息**对话框，发送测试消息。

    1.  在**分区**文本框，输入0。

    2.  在**Message Key**文本框，输入1。

    3.  在**Message Value**文本框，输入1。

    4.  单击**发送**。


## 查看函数日志

向消息队列Kafka版的数据源Topic发送消息后，查看函数日志，验证是否收到消息。详情请参见[配置并查看函数日志]()

日志中显示发送的测试消息。

![fc LOG](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4906119951/p127831.png)

