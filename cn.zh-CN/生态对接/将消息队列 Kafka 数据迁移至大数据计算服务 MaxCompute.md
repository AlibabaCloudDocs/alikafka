# 将消息队列 Kafka 数据迁移至大数据计算服务 MaxCompute {#task_1564026 .task}

本文介绍如何使用 DataWorks 数据同步功能，将消息队列 Kafka 集群上的数据迁移至阿里云大数据计算服务 MaxCompute，方便您对离线数据进行分析加工。

在开始本教程前，确保您已完成以下操作：

-   确保消息队列 Kafka 集群运行正常。本文以部署在华东1（杭州）地域（Region）的集群为例。
-   [开通 MaxCompute](../../../../cn.zh-CN/准备工作/开通MaxCompute.md#)。
-   [开通 DataWorks](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.18.7977417dyTrwFG&commodityCode=dide_create_post#/buy) 。
-   [创建项目](../../../../cn.zh-CN/准备工作/创建项目.md#)。本文以在华东1（杭州）地域创建名为 **bigdata\_DOC** 的项目为例。

示例如下。

![项目示例](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678854677_zh-CN.png)

## 背景信息 {#section_esa_2d2_9ps .section}

大数据计算服务 MaxCompute（原 ODPS）是一种大数据计算服务，能提供快速、完全托管免运维的 EB 级云数据仓库解决方案。

DataWorks 是基于 MaxCompute 计算和存储，提供工作流可视化开发、调度运维托管的一站式海量数据离线加工分析平台。在数加（一站式大数据平台）中，DataWorks 控制台即为 MaxCompute 控制台。MaxCompute 和 DataWorks 一起向用户提供完善的 ETL 和数仓管理能力，以及 SQL、MR、Graph 等多种经典的分布式计算模型，能够更快速地解决用户海量数据计算问题，有效降低企业成本，保障数据安全。

本教程旨在帮助您使用 DataWorks，将消息队列 Kafka 中的数据导入至 MaxCompute，来进一步探索大数据的价值。

## 步骤一：准备 Kafka 数据 {#section_q9y_fu0_hzj .section}

1.  登录[消息队列 Kafka 控制台](http://kafka.console.aliyun.com/)创建 Topic 和 Consumer Group，分别命名为 **testkafka** 和 **console-consumer**。具体步骤参见[步骤三：创建资源](../../../../cn.zh-CN/快速入门/步骤三：创建资源.md#)。本示例中，Consumer Group **console-consumer** 将用于消费 Topic **testkafka** 中的数据。
2.  向 Topic **testkafka** 中写入数据。由于 Kafka 用于处理流式数据，您可以持续不断地向其中写入数据。为保证测试结果，建议您写入 10 条以上的数据。您可以直接在控制台使用发送消息功能来写入数据，也可以使用消息队列 Kafka 的 SDK 收发消息。详情参见[使用 SDK 收发消息](../../../../cn.zh-CN/快速入门/步骤四：使用 SDK 收发消息/VPC 接入.md#)。
3.  为验证写入数据生效，您可以在控制台[查询消息](../../../../cn.zh-CN/用户指南/控制台使用指南/查询消息.md#)，看到之前写入 Topic 中的数据。

## 步骤二：创建 DataWorks 表 {#section_tgs_3vu_dpj .section}

您需创建 DataWorks 表，以保证大数据计算服务 MaxCompute 可以顺利接收消息队列 Kafka 数据。本例中为测试便利，使用非分区表。

1.  登录 [DataWorks 控制台](https://workbench.data.aliyun.com/consolenew#/)，在**工作空间**区域，单击目标工作空间的**进入数据开发**。
2.  在左侧导航栏单击**表管理**，然后单击新建图标。![创建表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678854678_zh-CN.png)


3.  在**新建表**对话框，输入表名 **testkafka**，然后单击**提交**。
4.  在创建的表页面，单击 **DDL模式**。
5.  在 **DDL模式**对话框，输入以下建表语句，单击**生成表结构**。 

    ``` {#codeblock_42o_oar_v5l}
    CREATE TABLE `testkafka` (
        `key` string,
        `value` string,
        `partition1` string,
        `timestamp1` string,
        `offset` string,
        `t123` string,
        `event_id` string,
        `tag` string
    ) ;
    ```

    建表语句中的每一列对应 DataWorks 数据集成 Kafka Reader 的默认列。

    -   key：表示消息的 Key。
    -   value：表示消息的完整内容 。
    -   partition：表示当前消息所在分区。
    -   headers：表示当前消息 headers 信息。
    -   offset：表示当前消息的偏移量。
    -   timestamp：表示当前消息的时间戳。
    您还可以自主命名，详情参见[配置 Kafka Reader](../../../../cn.zh-CN/使用指南/数据集成/作业配置/配置Reader插件/配置Kafka Reader.md#)。

6.  单击**提交到生产环境**。 详情请参见[表管理](../../../../cn.zh-CN/使用指南/数据开发/表管理.md#)。

## 步骤三：同步数据 {#section_my2_61l_bia .section}

1.  [新增任务资源](../../../../cn.zh-CN/使用指南/数据集成/常见配置/新增任务资源.md#)。此处创建的 ECS 实例将用以完成数据同步任务。
2.  登录 [DataWorks 控制台](https://workbench.data.aliyun.com/consolenew#/)，在**工作空间**区域，单击目标工作空间的**进入数据开发**。
3.  在左侧导航栏，选择**数据开发** \> **业务流程** \> **数据迁移**。
4.  右键选择**数据集成** \> **新建数据集成节点** \> **数据同步**。
5.  在**新建节点**对话框，输入节点名称（即数据同步任务名称），然后单击**提交**。
6.  在创建的节点页面，选择**数据来源**的**数据源**为 **Kafka** ，选择**数据去向**的**数据源**为**ODPS**，选择您在[步骤二：创建 DataWorks 表](#section_tgs_3vu_dpj)中创建的表。完成上述配置后，请单击框中的按钮，转换为脚本模式，如下图所示。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678854679_zh-CN.png)


7.  配置脚本，示例如下。 

    ``` {#codeblock_pr9_7qz_8f1}
    {
        "type": "job",
        "steps": [
            {
                "stepType": "kafka",
                "parameter": {
                    "server": "47.xxx.xxx.xxx:9092",
                    "kafkaConfig": {
                        "group.id": "console-consumer"
                    },
                    "valueType": "ByteArray",
                    "column": [
                        "__key__",
                        "__value__",
                        "__partition__",
                        "__timestamp__",
                        "__offset__",
                        "'123'",
                        "event_id",
                        "tag.desc"
                    ],
                    "topic": "testkafka",
                    "keyType": "ByteArray",
                    "waitTime": "10",
                    "beginOffset": "0",
                    "endOffset": "3"
                },
                "name": "Reader",
                "category": "reader"
            },
            {
                "stepType": "odps",
                "parameter": {
                    "partition": "",
                    "truncate": true,
                    "compress": false,
                    "datasource": "odps_first",
                    "column": [
                        "key",
                        "value",
                        "partition1",
                        "timestamp1",
                        "offset",
                        "t123",
                        "event_id",
                        "tag"
                    ],
                    "emptyAsNull": false,
                    "table": "testkafka"
                },
                "name": "Writer",
                "category": "writer"
            }
        ],
        "version": "2.0",
        "order": {
            "hops": [
                {
                    "from": "Reader",
                    "to": "Writer"
                }
            ]
        },
        "setting": {
            "errorLimit": {
                "record": ""
            },
            "speed": {
                "throttle": false,
                "concurrent": 1
            }
        }
    }
    ```

8.  在脚本页面，单击**配置任务资源组**，选择[步骤 1](#step_hxp_1vm_y46) 中创建的自定义资源组，然后单击**运行**图标。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678854680_zh-CN.png)


完成运行后，**运行日志**中显示运行成功。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678954607_zh-CN.png)

您可以新建一个数据开发任务运行 SQL 语句，查看当前表中是否已存在从 Kafka 同步过的数据。本文以`select * from testkafka`为例，具体步骤如下：

1.  在左侧导航栏，选择**数据开发** \> **业务流程**。
2.  右键选择**数据开发** \> **新建数据开发节点** \> **ODPS SQL**。
3.  在**新建节点**对话框，输入节点名称，然后单击**提交**。
4.  在创建的节点页面，输入`select * from testkafka`，然后单击**运行**图标。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1240818/156697678954608_zh-CN.png)

