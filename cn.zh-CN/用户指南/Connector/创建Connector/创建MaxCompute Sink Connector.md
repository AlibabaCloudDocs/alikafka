---
keyword: [kafka, connector, maxcompute]
---

# 创建MaxCompute Sink Connector

本文说明如何创建MaxCompute Sink Connector将数据从实例的数据源Topic导出至MaxCompute的表。

在创建MaxCompute Sink Connector前，请确保您已完成以下操作：

-   为实例开启Connector。更多信息，请参见[t1908611.md\#](/cn.zh-CN/用户指南/Connector/开启Connector.md)。
-   为实例创建数据源Topic。更多信息，请参见[t998824.md\#section\_zm0\_ysj\_343](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

    本文以名称为maxcompute-test-input的Topic为例。

-   通过MaxCompute客户端创建表。更多信息，请参见[t11950.md\#](/cn.zh-CN/快速入门/通过MaxCompute客户端使用MaxCompute/创建表.md)。

    本文以名称为connector\_test的项目下名称为test\_kafka的表为例。该表的建表语句如下：

    ```
    CREATE TABLE IF NOT EXISTS test_kafka(topic STRING,partition BIGINT,offset BIGINT,key STRING,value STRING) PARTITIONED by (pt STRING);
    ```


## 操作流程

使用MaxCompute Sink Connector将数据从实例的数据源Topic导出至MaxCompute的表操作流程如下：

1.  授予访问MaxCompute的权限。
    -   [创建RAM角色](#section_e02_70i_3jg)
    -   [添加权限](#section_rbd_pjy_5dh)
2.  创建MaxCompute Sink Connector依赖的Topic和Consumer Group

    如果您不需要自定义Topic和Consumer Group的名称，您可以直接跳过该步骤，在下一步骤选择自动创建。

    **说明：** 部分MaxCompute Sink Connector依赖的Topic的存储引擎必须为Local存储，大版本为0.10.2的实例不支持手动创建Local存储的Topic，只支持自动创建。

    1.  [创建MaxCompute Sink Connector依赖的Topic](#section_jvw_8cp_twy)
    2.  [创建MaxCompute Sink Connector依赖的Consumer Group](#section_xu7_scc_88s)
3.  [创建并部署MaxCompute Sink Connector](#section_mjv_rqc_6ds)
4.  结果验证
    1.  [发送测试消息](#section_idc_z6c_c33)
    2.  [查看表数据](#section_l1n_2qx_7kl)

## 创建RAM角色

由于RAM角色不支持直接选择作为受信服务，您在创建RAM角色时，需要选择任意支持的服务作为受信服务。RAM角色创建后，手动修改信任策略。

1.  登录[访问控制控制台](https://ram.console.aliyun.com/)。

2.  在左侧导航栏，单击**RAM角色管理**。

3.  在**RAM角色管理**页面，单击**创建RAM角色**。

4.  在**创建RAM角色**面板，执行以下操作。

    1.  **当前可信实体类型**区域，选择**阿里云服务**，然后单击**下一步**。

    2.  在**角色类型**区域，选择**普通服务角色**，在**角色名称**文本框，输入AliyunKafkaMaxComputeUser1，从**选择受信服务**列表，选择**大数据计算服务**，然后单击**完成**。

5.  在**RAM角色管理**页面，找到**AliyunKafkaMaxComputeUser1**，单击**AliyunKafkaMaxComputeUser1**。

6.  在**AliyunKafkaMaxComputeUser1**页面，单击**信任策略管理**页签，单击**修改信任策略**。

7.  在**修改信任策略**面板，将脚本中**fc**替换为alikafka，单击**确定**。

    ![pg_ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1686665061/p183450.png)


## 添加权限

为使Connector将消息同步到MaxCompute表，您需要为创建的RAM角色至少授予以下权限：

|客体|操作|描述|
|--|--|--|
|Project|CreateInstance|在项目中创建实例。|
|Table|Describe|读取表的元信息。|
|Table|Alter|修改表的元信息或添加删除分区。|
|Table|Update|覆盖或添加表的数据。|

关于以上权限的详细说明以及授权操作，请参见[t12097.md\#](/cn.zh-CN/安全管理/安全管理详解/用户及授权管理/授权.md)。

为本文创建的AliyunKafkaMaxComputeUser1添加权限的示例步骤如下：

1.  登录MaxCompute客户端。

2.  执行以下命令添加RAM角色为用户。

    ```
    add user `RAM$<accountid>:role/aliyunkafkamaxcomputeuser1`;
    ```

    **说明：** 将<accountid\>替换为您自己的阿里云账号ID。

3.  为RAM用户授予访问MaxCompute所需的最小权限。

    1.  执行以下命令为RAM用户授予项目相关权限。

        ```
        grant CreateInstance on project connector_test to user `RAM$<accountid>:role/aliyunkafkamaxcomputeuser1`;
        ```

        **说明：** 将<accountid\>替换为您自己的阿里云账号ID。

    2.  执行以下命令为RAM用户授予表相关权限。

        ```
        grant Describe, Alter, Update on table test_kafka to user `RAM$<accountid>:role/aliyunkafkamaxcomputeuser1`;
        ```

        **说明：** 将<accountid\>替换为您自己的阿里云账号ID。


## 创建MaxCompute Sink Connector依赖的Topic

您可以在控制台手动创建MaxCompute Sink Connector依赖的5个Topic，包括：任务位点Topic、任务配置Topic、任务状态Topic、死信队列Topic以及异常数据Topic。每个Topic所需要满足的分区数与存储引擎会有差异，具体信息，请参见[表 1](#table_y1t_x93_65g)。


## 创建MaxCompute Sink Connector依赖的Consumer Group

您可以在控制台手动创建MaxCompute Sink Connector数据同步任务使用的Consumer Group。该Consumer Group的名称必须为connect-任务名称，具体信息，请参见[表 1](#table_y1t_x93_65g)。


## 创建并部署MaxCompute Sink Connector

创建并部署用于将数据从同步至MaxCompute的MaxCompute Sink Connector。

1.  在**创建 Connector**配置向导面页面，完成以下操作。

    1.  在**配置基本信息**页签，按需配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**名称**|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-maxcompute-sink|
        |**实例**|默认配置为实例的名称与实例ID。|demo alikafka\_post-cn-st21p8vj\*\*\*\*|

    2.  在**配置源服务**页签，选择数据源为消息队列Kafka版，并配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**数据源 Topic**|需要同步数据的Topic。|maxcompute-test-input|
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
        |**Connector 消费组**|Connector的数据同步任务使用的Consumer Group。单击**配置运行环境**显示该参数。该Consumer Group的名称必须为connect-任务名称。|connect-kafka-maxcompute-sink|
        |**任务位点 Topic**|用于存储消费位点的Topic。单击**配置运行环境**显示该参数。        -   Topic名称：建议以connect-offset开头。
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-maxcompute-sink|
        |**任务配置 Topic**|用于存储任务配置的Topic。单击**配置运行环境**显示该参数。        -   Topic名称：建议以connect-config开头。
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-maxcompute-sink|
        |**任务状态 Topic**|用于存储任务状态的Topic。单击**配置运行环境**显示该参数。        -   Topic名称：建议以connect-status开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-maxcompute-sink|
        |**死信队列 Topic**|用于存储Connect框架的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-maxcompute-sink|
        |**异常数据 Topic**|用于存储Sink的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic名称：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-maxcompute-sink|

    3.  在**配置目标服务**页签，选择目标服务为大数据计算服务，并配置以下参数，然后单击**创建**。

        |参数|描述|示例值|
        |--|--|---|
        |**连接地址**|MaxCompute的服务接入点。更多信息，请参见[t11949.md\#](/cn.zh-CN/准备工作/配置Endpoint.md)。         -   VPC网络Endpoint：低延迟，推荐。适用于实例和MaxCompute处于同一地域场景。
        -   外网Endpoint：高延迟，不推荐。适用于实例和MaxCompute处于不同地域的场景。如需使用公网Endpoint，您需要为Connector开启公网访问。更多信息，请参见[t1910210.md\#](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。
|http://service.cn-hangzhou.maxcompute.aliyun-inc.com/api|
        |**工作空间**|MaxCompute的工作空间。|connector\_test|
        |**表**|MaxCompute的表。|test\_kafka|
        |**表地域**|MaxCompute表所在地域。|华东1（杭州）|
        |**服务账号**|MaxCompute的阿里云账号ID。|188\*\*\*|
        |**授权角色名**|的RAM角色的名称。更多信息，请参见[创建RAM角色](#section_e02_70i_3jg)。|AliyunKafkaMaxComputeUser1|
        |**模式**|消息同步到Connector的模式。默认为DEFAULT。取值说明如下：        -   **KEY**：只保留消息的Key，并将Key写入MaxCompute表的key列。
        -   **VALUE**：只保留消息的Value，并将Value写入MaxCompute表的value列。
        -   **DEFAULT**：同时保留消息的Key和Value，并将Key和Value分别写入MaxCompute表的key列和value列。

**说明：** DEFAULT模式下，不支持选择CSV格式，只支持TEXT格式和BINARY格式。

|DEFAULT|
        |**格式**|消息同步到Connector的格式。默认为TEXT。取值说明如下：        -   **TEXT**：消息的格式为字符串。
        -   **BINARY**：消息的格式为字节数组。
        -   **CSV**：消息的格式为逗号（,）分隔的字符串。

**说明：** CSV格式下，不支持DEFAULT模式，只支持KEY模式和VALUE模式：

            -   KEY模式：只保留消息的Key，根据逗号（,）分隔Key字符串，并将分隔后的字符串按照索引顺序写入表。
            -   VALUE模式：只保留消息的Value，根据逗号（,）分隔Value字符串，并将分隔后的字符串按照索引顺序写入表。
|TEXT|
        |**分区**|分区的粒度。默认为HOUR。取值说明如下：        -   **DAY**：每天将数据写入一个新分区。
        -   **HOUR**：每小时将数据写入一个新分区。
        -   **MINUTE**：每分钟将数据写入一个新分区。
|HOUR|
        |**时区**|向Connector的数据源Topic发送消息的生产者客户端所在时区。默认为GMT 08:00。|GMT 08:00|

        创建完成后，在**Connector 管理**页面，查看创建的Connector 。

2.  创建完成后，在**Connector 管理**页面，找到创建的Connector ，单击其**操作**列的**部署**。


## 发送测试消息

部署MaxCompute Sink Connector后，您可以向的数据源Topic发送消息，测试数据能否被同步至MaxCompute。


## 查看表数据

向的数据源Topic发送消息后，在MaxCompute客户端查看表数据，验证是否收到消息。

查看本文写入的test\_kafka的示例步骤如下：

1.  登录MaxCompute客户端。

2.  执行以下命令查看表的数据分区。

    ```
    show partitions test_kafka;
    ```

    返回结果示例如下：

    ```
    pt=11-17-2020 15
    
    OK
    ```

3.  执行以下命令查看分区的数据。

    ```
    select * from test_kafka where pt ="11-17-2020 14";
    ```

    返回结果示例如下：

    ```
    +----------------------+------------+------------+-----+-------+---------------+
    | topic                | partition  | offset     | key | value | pt            |
    +----------------------+------------+------------+-----+-------+---------------+
    | maxcompute-test-input| 0          | 0          | 1   | 1     | 11-17-2020 14 |
    +----------------------+------------+------------+-----+-------+---------------+
    ```


