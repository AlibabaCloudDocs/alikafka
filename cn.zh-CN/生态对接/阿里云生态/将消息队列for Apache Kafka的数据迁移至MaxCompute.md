---
keyword: [Kafka, 大数据]
---

# 将消息队列for Apache Kafka的数据迁移至MaxCompute

本文介绍如何使用DataWorks数据集成，将消息队列for Apache Kafka集群上的数据迁移至MaxCompute，方便您对离线数据进行分析加工。

-   消息队列 for Apache Kafka集群运行正常。本文以部署在华东1（杭州）地域的集群为例。
-   [开通 MaxCompute](/cn.zh-CN/准备工作/开通MaxCompute.md)。
-   [开通 DataWorks](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.18.7977417dyTrwFG&commodityCode=dide_create_post#/buy) 。
-   [创建项目空间](/cn.zh-CN/准备工作/创建项目空间.md)。本文以在华东1（杭州）地域创建名为bigdata\_DOC的项目为例。

## 步骤一：准备Kafka数据

1.  登录[消息队列 for Apache Kafka控制台](http://kafka.console.aliyun.com/)创建Topic和Consumer Group，分别命名为testkafka和console-consumer。详情请参见[步骤三：创建资源](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

    本示例中， console-consumer将用于消费testkafka中的数据。

2.  向testkafka中写入数据。由于消息队列 for Apache Kafka用于处理流式数据，您可以持续不断地向其中写入数据。为保证测试结果，建议写入10 条以上的数据。您可以直接在控制台使用发送消息功能来写入数据，也可以使用消息队列 for Apache Kafka的SDK收发消息。详情参见[使用SDK收发消息](/cn.zh-CN/快速入门/步骤四：使用SDK收发消息/默认接入点收发消息.md)。

3.  为验证写入数据是否生效，您可以在控制台查询消息，查看之前写入Topic中的数据。详情请参见[查询消息](/cn.zh-CN/用户指南/查询消息.md)。


## 步骤二：在DataWorks上创建目标表

您需要在DataWorks上创建目标表，以保证MaxCompute可以接收消息队列for Apache Kafka的数据。

1.  进入**数据开发**页面。

    1.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

    2.  在左侧导航栏，单击**工作空间列表**。

    3.  单击相应工作空间后的**进入数据开发**。

2.  右键单击**业务流程**，选择**新建** \> **MaxCompute** \> **表**。

3.  在表的编辑页面，单击**DDL模式**。

4.  在DDL模式对话框，输入如下建表语句，单击**生成表结构**。

    ```
    CREATE TABLE testkafka
    (
     key            string,
     value          string,
     partition1     string,
     timestamp1     string,
     offset         string,
     t123           string,
     event_id       string,
     tag            string
    ) ;
    ```

    建表语句中的每一列对应DataWorks数据集成Kafka Reader的默认列：

    -   key：表示消息的 Key。
    -   value：表示消息的完整内容 。
    -   partition：表示当前消息所在分区。
    -   headers：表示当前消息 headers 信息。
    -   offset：表示当前消息的偏移量。
    -   timestamp：表示当前消息的时间戳。
    您还可以自主命名，详情参见[Kafka Reader]()。

5.  单击**提交到生产环境**并**确认**。


## 步骤三：同步数据

1.  [t1656370.md\#]()。此处创建的ECS实例将用以完成数据集成任务。

2.  新建数据集成节点。

    1.  进入数据开发页面，右键单击指定业务流程，选择**新建** \> **数据集成** \> **离线同步**。

    2.  在**新建节点**对话框中，输入**节点名称**，并单击**提交**。

3.  在顶部菜单栏上，单击![转化脚本](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8793359951/p100995.png)图标。

4.  在脚本模式下，单击顶部菜单栏上的![**](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8793359951/p101002.png)图标。

5.  在**导入模板**对话框中选择**来源类型**、**数据源**、**目标类型**及**数据源**。

6.  配置脚本，示例代码如下。

    ```
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

7.  配置调度资源组。

    1.  在右侧导航栏，单击**调度配置**。

    2.  在**资源属性**区域，选择**调度资源组**为[步骤 1](#step_hxp_1vm_y46) 中创建的自定义资源组。

8.  单击![**](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9993359951/p100471.png)图标运行代码。

9.  您可以在**运行日志**查看运行结果。


您可以新建一个ODPS SQL节点运行SQL语句，查看从Kafka同步数据至MaxCompute是否成功。详情请参见[使用临时查询运行SQL语句（可选）](/cn.zh-CN/快速入门/使用临时查询运行SQL语句（可选）.md)。

