# 创建Elasticsearch Sink Connector

本文介绍如何创建Elasticsearch Sink Connector将数据从消息队列Kafka版实例的数据源Topic导出至阿里云Elasticsearch的索引。

在导出数据前，请确保您已完成以下操作：

-   为消息队列Kafka版实例开启Connector。更多信息，请参见[开启Connector](/intl.zh-CN/用户指南/Connector/开启Connector.md)。
-   为消息队列Kafka版实例创建数据源Topic。更多信息，请参见[步骤一：创建Topic](/intl.zh-CN/快速入门/步骤三：创建资源.md)。
-   在[Elasticsearch管理控制台](https://elasticsearch.console.aliyun.com/)创建实例和索引。更多信息，请参见[快速开始](/intl.zh-CN/Elasticsearch/快速开始.md)。

    **说明：** 函数计算使用的Elasticsearch客户端版本为7.7.0，为保持兼容，您需创建7.0或以上版本的Elasticsearch实例。

-   [开通函数计算服务]()。

## 注意事项

-   仅支持在同地域内，将数据从消息队列Kafka版实例的数据源Topic导出至函数计算，再由函数计算导出至Elasticsearch。Connector的限制说明，请参见[使用限制](/intl.zh-CN/用户指南/Connector/Connector概述.md)。
-   该功能基于函数计算服务提供。函数计算为您提供了一定的免费额度，超额部分将产生费用，请以函数计算的计费规则为准。计费详情，请参见[计费概述]()。
-   函数计算的函数调用支持日志查询。具体操作步骤，请参见[配置并查看函数日志]()。
-   消息转储时，消息队列Kafka版中消息用UTF-8 String序列化，暂不支持二进制的数据格式。

## 创建并部署Elasticsearch Sink Connector

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Connector 管理**。

5.  在**Connector 管理**页面，单击**创建 Connector**。

6.  在**创建 Connector**配置向导面页面，完成以下操作。

    1.  在**配置基本信息**页签，按需配置以下参数，然后单击**下一步**。

        **说明：** 消息队列Kafka版会为您自动选中**授权创建服务关联角色**。

        -   如果未创建服务关联角色，消息队列Kafka版会为您自动创建一个服务关联角色，以便您使用消息队列Kafka版导出数据至Elasticsearch的功能。
        -   如果已创建服务关联角色，消息队列Kafka版不会重复创建。
        关于该服务关联角色的更多信息，请参见[服务关联角色](/intl.zh-CN/权限控制/服务关联角色.md)。

        |参数|描述|示例值|
        |--|--|---|
        |**名称**|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-elasticsearch-sink|
        |**实例**|默认配置为实例的名称与实例ID。|demo alikafka\_post-cn-st21p8vj\*\*\*\*|

    2.  在**配置源服务**页签，选择数据源为消息队列Kafka版，并配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**数据源 Topic**|需要同步数据的Topic。|elasticsearch-test-input|
        |**消费线程并发数**|数据源Topic的消费线程并发数。默认值为6。取值说明如下：        -   **6**
        -   **12**
|**6**|
        |**消费初始位置**|开始消费的位置。取值说明如下：         -   **最早位点**：从最初位点开始消费。
        -   **最近位点**：从最新位点开始消费。
|**最早位点**|
        |**VPC ID**|数据同步任务所在的VPC。单击**配置运行环境**显示该参数。默认为消息队列Kafka版实例所在的VPC，您无需填写。|vpc-bp1xpdnd3l\*\*\*|
        |**VSwitch ID**|数据同步任务所在的交换机。单击**配置运行环境**显示该参数。该交换机必须与消息队列Kafka版实例处于同一VPC。默认为部署消息队列Kafka版实例时填写的交换机。|vsw-bp1d2jgg81\*\*\*|
        |**失败处理**|消息发送失败后，是否继续订阅出现错误的Topic的分区。单击**配置运行环境**显示该参数。取值说明如下。        -   **继续订阅**：继续订阅出现错误的Topic的分区，并打印错误日志。
        -   **停止订阅**：停止订阅出现错误的Topic的分区，并打印错误日志
**说明：**

        -   如何查看日志，请参见[查看Connector日志](/intl.zh-CN/用户指南/Connector/查看Connector日志.md)。
        -   如何根据错误码查找解决方案，请参见[错误码]()。
        -   如需恢复对出现错误的Topic的分区的订阅，您需要[提交工单](https://workorder-intl.console.aliyun.com/?spm=5176.kafka.aliyun_topbar.8.79e425e8DncGA9#/ticket/add/?productId=1352)联系消息队列Kafka版技术人员。
|**继续订阅**|
        |**创建资源方式**|选择创建Connector所依赖的Topic与Group的方式。单击**配置运行环境**显示该参数。        -   **自动创建**
        -   **手动创建**
|**自动创建**|
        |**Connector 消费组**|Connector的数据同步任务使用的Consumer Group。单击**配置运行环境**显示该参数。该Consumer Group的名称必须为connect-任务名称。|connect-kafka-elasticsearch-sink|
        |**任务位点 Topic**|用于存储消费位点的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-offset开头。
        -   分区数：Topic的分区数量必须大于1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-offset-kafka-elasticsearch-sink|
        |**任务配置 Topic**|用于存储任务配置的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-config开头。
        -   分区数：Topic的分区数量必须为1。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-config-kafka-elasticsearch-sink|
        |**任务状态 Topic**|用于存储任务状态的Topic。单击**配置运行环境**显示该参数。        -   Topic：建议以connect-status开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎必须为Local存储。
        -   cleanup.policy：Topic的日志清理策略必须为compact。
|connect-status-kafka-elasticsearch-sink|
        |**死信队列 Topic**|用于存储Connect框架的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和异常数据Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-elasticsearch-sink|
        |**异常数据 Topic**|用于存储Sink的异常数据的Topic。单击**配置运行环境**显示该参数。该Topic可以和死信队列Topic为同一个Topic，以节省Topic资源。        -   Topic：建议以connect-error开头。
        -   分区数：Topic的分区数量建议为6。
        -   存储引擎：Topic的存储引擎可以为Local存储或云存储。
|connect-error-kafka-elasticsearch-sink|

    3.  在**配置目标服务**页签，选择目标服务为对象存储，并配置以下参数，然后单击**创建**。

        |参数|描述|示例值|
        |--|--|---|
        |**ES 实例 ID**|阿里云Elasticsearch实例ID。|es-cn-oew1o67x0000\*\*\*\*|
        |**接入地址**|阿里云Elasticsearch实例的公网或私网地址。更多信息，请参见[查看实例的基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)。|es-cn-oew1o67x0000\*\*\*\*.elasticsearch.aliyuncs.com|
        |**接入端口**|访问阿里云Elasticsearch的公网或私网端口，取值如下：        -   9200：基于HTTP或HTTPS。
        -   9300：基于TCP。
更多信息，请参见[查看实例的基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)。

|9300|
        |**用户名**|登录Kibana控制台的用户名，默认为elastic。您也可以创建自定义用户，创建步骤，请参见[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|elastic|
        |**用户密码**|登录Kibana控制台的密码。elastic用户的密码在创建实例时设定，如果忘记可重置。如需重置，请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|\*\*\*\*\*\*\*\*|
        |**索引**|阿里云Elasticsearch的索引名称。|elastic\_test|

        **说明：**

        -   用户名和用户密码会被用来初始化Elasticsearch对象，通过bulk投递消息，请确认账号对索引有写权限。
        -   用户名和用户密码是消息队列Kafka版创建任务时作为环境变量传递至函数计算的函数，任务创建成功后，消息队列Kafka版不保存相关信息。
        创建完成后，在**Connector 管理**页面，查看创建的Connector 。

7.  创建完成后，在**Connector 管理**页面，找到创建的Connector ，单击其**操作**列的**部署**。


## 配置函数服务

您在消息队列Kafka版控制台成功创建并部署Elasticsearch Sink Connector后，函数计算会自动为您创建给该Connector使用的函数服务，服务命名格式为`kafka-service-<connector_name>-<随机String>`。

1.  在**Connector 管理**页面，找到目标Connector，在其右侧**操作**列，选择**更多** \> **查看日志**。

    页面跳转至函数计算控制台。

2.  在函数计算控制台，找到自动创建的函数服务，并配置其VPC和交换机信息。请确保该信息和您阿里云Elasticsearch相同。配置的具体步骤，请参见[更新服务]()。


## 发送测试消息

您可以向消息队列Kafka版的数据源Topic发送消息，测试数据能否被导出至阿里云Elasticsearch。

1.  在**Connector 管理**页面，找到目标Connector，在其右侧**操作**列，单击**测试**。

2.  在**发送消息**面板，发送测试消息。

    -   **发送方式**选择**控制台**。
        1.  在**消息 Key**文本框中输入消息的Key值，例如demo。
        2.  在**消息内容**文本框输入测试的消息内容，例如 \{"key": "test"\}。
        3.  设置**发送到指定分区**，选择是否指定分区。
            1.  单击**是**，在**分区 ID**文本框中输入分区的ID，例如0。如果您需查询分区的ID，请参见[查看分区状态](/intl.zh-CN/用户指南/Topic/查看分区状态.md)。
            2.  单击**否**，不指定分区。
    -   **发送方式**选择**Docker**，执行**运行 Docker 容器生产示例消息**区域的Docker命令，发送消息。
    -   **发送方式**选择**SDK**，根据您的业务需求，选择需要的语言或者框架的SDK以及接入方式，通过SDK发送消息。

## 验证结果

向消息队列Kafka版的数据源Topic发送消息后，[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)，执行`GET /<index_name>/_search`查看索引，验证数据导出结果。

消息队列Kafka版数据导出至Elasticsearch的格式示例如下：

```
{
  "took" : 8,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "product_****",
        "_type" : "_doc",
        "_id" : "TX3TZHgBfHNEDGoZ****",
        "_score" : 1.0,
        "_source" : {
          "msg_body" : {
            "key" : "test",
            "offset" : 2,
            "overflowFlag" : false,
            "partition" : 2,
            "timestamp" : 1616599282417,
            "topic" : "dv****",
            "value" : "test1",
            "valueSize" : 8
          },
          "doc_as_upsert" : true
        }
      }
    ]
  }
}
```

