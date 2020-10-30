---
keyword: [kafka, connector, maxcompute]
---

# 创建MaxCompute Sink Connector

本文说明如何创建MaxCompute Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至MaxCompute的表。

在创建MaxCompute Sink Connector前，请确保您已完成以下操作：

1.  为消息队列Kafka版实例开启Connector。详情请参见[开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)。
2.  为消息队列Kafka版创建数据源Topic。详情请参见[步骤一：创建Topic](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

    本文以名称为maxcompute-test-input的Topic为例。

3.  通过MaxCompute客户端创建表。详情请参见[创建和查看表](/cn.zh-CN/快速入门/创建和查看表.md)。

    本文以名称为test\_kafka的表为例。该示例表的建表语句如下：

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,`partition` BIGINT,`offset` BIGINT,key STRING,value STRING,dt DATETIME) STORED AS ALIORC TBLPROPERTIES ('comment'='');
    ```


## 操作流程

使用MaxCompute Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至MaxCompute的表操作流程如下：

1.  创建MaxCompute Sink Connector依赖的Topic和Consumer Group（可选）

    如果您不需要自定义Topic和Consumer Group的名称，您可以直接跳过该步骤，在下一步骤选择自动创建。

    **说明：** 部分MaxCompute Sink Connector依赖的Topic的存储引擎必须为Local存储，大版本为0.10.2的消息队列Kafka版实例不支持手动创建Local存储的Topic，只支持自动创建。

    1.  [创建MaxCompute Sink Connector依赖的Topic（可选）](#section_jvw_8cp_twy)
    2.  [创建MaxCompute Sink Connector依赖的Consumer Group（可选）](#section_xu7_scc_88s)
2.  创建并部署MaxCompute Sink Connector
    1.  [创建MaxCompute Sink Connector](#section_mjv_rqc_6ds)
    2.  [部署MaxCompute Sink Connector](#section_444_q49_c46)
3.  结果验证
    1.  [发送消息](#section_idc_z6c_c33)
    2.  [查看表数据](#section_l1n_2qx_7kl)

## 创建MaxCompute Sink Connector依赖的Topic（可选）

您可以在消息队列Kafka版控制台手动创建MaxCompute Sink Connector依赖的5个Topic。

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


## 创建MaxCompute Sink Connector依赖的Consumer Group（可选）

您可以在消息队列Kafka版控制台手动创建MaxCompute Sink Connector依赖的2个Consumer Group。

1.  在左侧导航栏，单击**Consumer Group管理**。

2.  在**Consumer Group管理**页面，选择实例，单击**创建Consumer Group**。

3.  在**创建Consumer Group**对话框，设置Topic属性，然后单击**创建**。

    |Consumer Group|描述|
    |--------------|--|
    |Connector任务消费组|Connector的数据同步任务使用的Consumer Group。该Consumer Group的名称必须为connect-任务名称。|
    |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|


## 创建MaxCompute Sink Connector

创建用于将数据从消息队列Kafka版同步至MaxCompute的MaxCompute Sink Connector。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在**Connector**页面，选择实例，单击**创建Connector**。

4.  在**创建Connector**面板，完成以下操作。

    1.  在**基础信息**下方的**Connector名称**文本框，输入Connector名称，从**转储路径**列表，选择**消息队列Kafka版**，从**转储到**列表，选择**MaxCompute**，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |Connector名称|Connector的名称。取值        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-maxcompute-sink|
        |任务类型|Connector的数据同步任务类型。本文以数据从消息队列Kafka版同步至函数计算为例。更多任务类型，请参见[Connector类型](/cn.zh-CN/用户指南/Connector/Connector概述.md)。|KAFKA2ODPS|

    2.  在**源实例配置**下方的**数据源Topic**文本框，输入数据源Topic名称，从**消费初始位置**列表，选择消费初始位置，在**创建资源**，选择**自动创建**或者选择**手动创建**并输入手动创建的Topic的名称，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |VPC ID|数据同步任务所在的VPC。默认为消息队列Kafka版实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |VSwitch ID|数据同步任务所在的交换机。用户交换机必须与消息队列Kafka版实例处于同一VPC。默认为部署消息队列Kafka版实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |数据源Topic|需要同步数据的Topic。|maxcompute-test-input|
        |消费初始位置|开始消费的位置。取值：         -   latest：从最新位点开始消费。
        -   earliest：从最初位点开始消费。
|latest|
        |Connector消费组|Connector使用的Consumer Group。该Consumer Group的名称建议以connect-cluster开头。|connect-cluster-kafka-maxcompute-sink|
        |任务位点Topic|用于存储消费位点的Topic。        -   Topic名称：建议以connect-offset开头
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-maxcompute-sink|
        |任务配置Topic|用于存储任务配置的Topic。        -   Topic名称：建议以connect-config开头
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-maxcompute-sink|
        |任务状态Topic|用于存储任务状态的Topic。        -   Topic名称：建议以connect-status开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-maxcompute-sink|
        |死信队列Topic|用于存储Connect框架的异常数据的Topic。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-maxcompute-sink|
        |异常数据Topic|用于存储Sink的异常数据的Topic。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-maxcompute-sink|

    3.  在**目标实例配置**下方，输入MaxCompute的属性，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |MaxCompute连接地址|MaxCompute的服务接入点。详情请参见[配置Endpoint](/cn.zh-CN/准备工作/配置Endpoint.md)。         -   VPC网络Endpoint：低延迟，推荐。适用于消息队列Kafka版实例和MaxCompute处于同一地域场景。
        -   外网Endpoint：高延迟，不推荐。适用于消息队列Kafka版实例和MaxCompute处于不同地域的场景。如需使用公网Endpoint，您需要为Connector开启公网访问。详情请参见[为Connector开启公网访问](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
        |MaxCompute工作空间|MaxCompute的工作空间。|connector\_test|
        |MaxCompute表|MaxCompute的表。|test\_kafka|
        |阿里云账号ID|MaxCompute所属阿里云账号的AccessKey ID。|LTAI4F\*\*\*|
        |阿里云账号Key|MaxCompute所属阿里云账号的AccessKey Secret。|wvDxjjR\*\*\*|

5.  在**预览/创建**下方，确认Connector的配置，然后单击**提交**。

    提交完成后，刷新**Connector**页面以显示创建的Connector。


## 部署MaxCompute Sink Connector

创建MaxCompute Sink Connector后，您需要部署该MaxCompute Sink Connector以使其将数据从消息队列Kafka版同步至MaxCompute。

1.  在**Connector**页面，找到创建的MaxCompute Sink Connector，在其右侧**操作**列，单击**部署**。

    部署完成后，Connector状态显示运行中。


## 发送消息

部署MaxCompute Sink Connector后，您可以向消息队列Kafka版的数据源Topic发送消息，测试数据能否被同步至MaxCompute。

1.  在左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，选择实例，找到**maxcompute-test-input**，在其右侧**操作**列，单击**发送消息**。

3.  在**发送消息**对话框，发送测试消息。

    1.  在**分区**文本框，输入0。

    2.  在**Message Key**文本框，输入1。

    3.  在**Message Value**文本框，输入1。

    4.  单击**发送**。


## 查看表数据

向消息队列Kafka版的数据源Topic发送消息后，在MaxCompute客户端查看表数据，验证是否收到消息。

1.  [安装并配置客户端](/cn.zh-CN/准备工作/安装并配置客户端.md)。

2.  执行以下命令查看表test\_kafka的数据。

    ```
    SELECT COUNT(*) FROM test_kafka;
    ```

    返回的表数据中显示发送的测试消息。

    ![table_result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7906119951/p127744.png)


