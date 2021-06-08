---
keyword: [kafka, elasticsearch, vpc]
---

# 将消息队列Kafka版接入阿里云Elasticsearch

随着时间的积累，消息队列Kafka版中的日志数据会越来越多。当您需要查看并分析庞杂的日志数据时，可通过阿里云Logstash将消息队列Kafka版中的日志数据导入阿里云Elasticsearch，然后通过Kibana进行可视化展示与分析。本文介绍将消息队列Kafka版接入阿里云Elasticsearch的操作方法。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。具体信息，请参见[VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)。
-   创建阿里云Elasticsearch实例。具体信息，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

    **说明：** 请注意保存创建阿里云Elasticsearch实例时设置的用户名及密码。该用户名及密码将用于[步骤五：创建索引](#section_awp_hjm_4to)、[步骤六：创建管道](#section_x33_eux_vin)和[步骤七：搜索数据](#section_0lt_6hh_dfu)。

-   创建阿里云Logstash实例。具体信息，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

通过阿里云Logstash将数据从消息队列Kafka版导入阿里云Elasticsearch的过程如下图所示。

![elasticsearch](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240235951/p111467.png)

-   消息队列Kafka版

    消息队列Kafka版是阿里云提供的分布式、高吞吐、可扩展的消息队列服务。消息队列Kafka版广泛用于日志收集、监控数据聚合、流式数据处理、在线和离线分析等大数据领域，已成为大数据生态中不可或缺的部分。更多信息，请参见[什么是消息队列Kafka版](/cn.zh-CN/产品简介/什么是消息队列Kafka版？.md)。

-   阿里云Elasticsearch

    Elasticsearch简称ES，是一个基于Lucene的实时分布式的搜索与分析引擎，是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。它提供了一个分布式服务，可以使您快速的近乎于准实时的存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术。阿里云Elasticsearch支持5.5.3、6.3.2、6.7.0、6.8.0和7.4.0版本，并提供了商业插件X-Pack服务，致力于数据分析、数据搜索等场景服务。在开源Elasticsearch的基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。更多信息，请参见[什么是阿里云Elasticsearch](/cn.zh-CN/产品简介/什么是阿里云Elasticsearch.md)。

-   阿里云Logstash

    阿里云Logstash作为服务器端的数据处理管道，提供了100%兼容开源的Logstash功能。Logstash能够动态地从多个来源采集数据、转换数据，并且将数据存储到所选择的位置。通过输入、过滤和输出插件，Logstash可以对任何类型的事件加工和转换。更多信息，请参见[什么是阿里云Logstash](/cn.zh-CN/Logstash/什么是阿里云Logstash.md)。


## 步骤一：获取VPC环境接入点

阿里云Logstash通过消息队列Kafka版的接入点与消息队列Kafka版在VPC环境下建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在**实例详情**页面的**接入点信息**页签，获取实例的VPC环境接入点。

    ![endpoint](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2804172261/p111363.png)

    消息队列Kafka版支持以下VPC环境接入点：

    -   默认接入点：端口号为9092。
    -   SASL接入点：端口号为9094。如需使用SASL接入点，请开启ACL。您可以[提交工单](https://selfservice.console.aliyun.com/#/ticket/category/alikafka)申请开启ACL。
    更多信息，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

    **说明：** Topic需要在应用程序所在的地域（即所部署的ECS的所在地域）进行创建。Topic不能跨地域使用。例如Topic创建在华北2（北京）这个地域，那么消息生产端和消费端也必须运行在华北2（北京）的ECS。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Topic 管理**。

5.  在**Topic 管理**页面，单击**创建 Topic**。

6.  在**创建 Topic**面板，设置Topic属性，然后单击**确定**。

    ![创建Topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8808912261/p278627.png)

    |参数|说明|示例|
    |--|--|--|
    |**名称**|Topic名称。|demo|
    |**描述**|Topic的简单描述。|demo test|
    |**分区数**|Topic的分区数量。|12|
    |**存储引擎**|Topic消息的存储引擎。 消息队列Kafka版支持以下两种存储引擎。

     -   **云存储**：底层接入阿里云云盘，具有低时延、高性能、持久性、高可靠等特点，采用分布式3副本机制。实例的**规格类型**为**标准版（高写版）**时，存储引擎只能为**云存储**。
    -   **Local 存储**：使用原生Kafka的ISR复制算法，采用分布式3副本机制。
|**云存储**|
    |**消息类型**|Topic消息的类型。     -   **普通消息**：默认情况下，保证相同Key的消息分布在同一个分区中，且分区内消息按照发送顺序存储。集群中出现机器宕机时，可能会造成消息乱序。当**存储引擎**选择**云存储**时，默认选择**普通消息**。
    -   **分区顺序消息**：默认情况下，保证相同Key的消息分布在同一个分区中，且分区内消息按照发送顺序存储。集群中出现机器宕机时，仍然保证分区内按照发送顺序存储。但是会出现部分分区发送消息失败，等到分区恢复后即可恢复正常。当**存储引擎**选择**Local 存储**时，默认选择**分区顺序消息**。
|**普通消息**|
    |**日志清理策略**|Topic日志的清理策略。 当**存储引擎**选择**Local 存储**时，需要配置**日志清理策略**。

 消息队列Kafka版支持以下两种日志清理策略。

     -   **Delete**：默认的消息清理策略。在磁盘容量充足的情况下，保留在最长保留时间范围内的消息；在磁盘容量不足时（一般磁盘使用率超过85%视为不足），将提前删除旧消息，以保证服务可用性。
    -   **Compact**：使用[Kafka Log Compaction日志清理策略](https://kafka.apache.org/documentation/?spm=a2c4g.11186623.2.15.1cde7bc3c8pZkD#compaction)。Log Compaction清理策略保证相同Key的消息，最新的value值一定会被保留。主要适用于系统宕机后恢复状态，系统重启后重新加载缓存等场景。例如，在使用Kafka Connect或Confluent Schema Registry时，需要使用Kafka Compact Topic存储系统状态信息或配置信息。

**说明：** Compact Topic一般只用在某些生态组件中，例如Kafka Connect或Confluent Schema Registry，其他情况的消息收发请勿为Topic设置该属性。具体信息，请参见[消息队列Kafka版Demo库](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master)。

|**Compact**|
    |**标签**|Topic的标签。|demo|

    创建完成后，在**Topic 管理**页面的列表中显示已创建的Topic。


## 步骤三：发送消息

向创建的Topic发送消息。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4e.11153940.0.0.473e500dpMSQGl#/TopicManagement?regionId=cn-hangzhou&instanceId=alikafka_pre-cn-4591fbkd400a)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Topic 管理**。

5.  在**Topic 管理**页面，找到目标Topic，在其**操作**列中，选择**更多** \> **体验发送消息**。

6.  在**快速体验消息收发**面板，发送测试消息。

    -   **发送方式**选择**控制台**。
        1.  在**消息 Key**文本框中输入消息的Key值，例如demo。
        2.  在**消息内容**文本框输入测试的消息内容，例如 \{"key": "test"\}。
        3.  设置**发送到指定分区**，选择是否指定分区。
            1.  单击**是**，在**分区 ID**文本框中输入分区的ID，例如0。如果您需查询分区的ID，请参见[查看分区状态](/cn.zh-CN/用户指南/Topic/查看分区状态.md)。
            2.  单击**否**，不指定分区。
        4.  根据界面提示信息，通过SDK订阅消息，或者执行Docker命令订阅消息。
    -   **发送方式**选择**Docker**，运行Docker容器。
        1.  执行**运行 Docker 容器生产示例消息**区域的Docker命令，发送消息。
        2.  执行**发送后如何消费消息？**区域的Docker命令，订阅消息。
    -   **发送方式**选择**SDK**，根据您的业务需求，选择需要的语言或者框架的SDK以及接入方式，通过SDK体验消息收发。

## 步骤四：创建Consumer Group

创建阿里云Elasticsearch所属的Consumer Group。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Group 管理**。

5.  在**Group 管理**页面，单击**创建 Group**。

6.  在**创建 Group**面板的**Group ID**文本框输入Group的名称，在**描述**文本框简要描述Group，并给Group添加标签，单击**确定**。

    创建完成后，在**Group 管理**页面的列表中显示已创建的Group。


## 步骤五：创建索引

通过阿里云Elasticsearch创建索引，接收消息队列Kafka版的数据。

1.  登录[阿里云Elasticsearch控制台](elasticsearch.console.aliyun.com)。

2.  在顶部菜单栏，选择地域。

3.  在**实例列表**页面，单击创建的实例。

4.  在左侧导航栏，单击**可视化控制**。

5.  在**Kibana**区域，单击**进入控制台**。

6.  在Kibana登录页面，输入Username和Password，然后单击**Log in**。

    **说明：**

    -   Username为您创建阿里云Elasticsearch实例时设置的用户名。
    -   Password为您创建阿里云Elasticsearch实例时设置的密码。
7.  在Kibana控制台的左侧导航栏，单击**Dev Tools**。

8.  执行以下命令创建索引。

    ```
    PUT /elastic_test
    {}
    ```


## 步骤六：创建管道

通过阿里云Logstash创建管道。管道部署后，将源源不断地从消息队列Kafka版导入数据进阿里云Elasticsearch。

1.  登录[阿里云Logstash控制台](https://elasticsearch.console.aliyun.com/#/logstashes)。

2.  在顶部菜单栏，选择地域。

3.  在**实例列表**页面，单击创建的实例。

4.  在左侧导航栏，单击**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

6.  在**Config配置**中，输入配置。

    配置示例如下。

    ```
    input {
        kafka {
        bootstrap_servers => ["192.168.xx.xx:9092,192.168.xx.xx:9092,192.168.xx.xx:9092"]
        group_id => "elastic_group"
        topics => ["elastic_test"]
        consumer_threads => 12
        decorate_events => true
        }
    }
    output {
        elasticsearch {
        hosts => ["http://es-cn-o40xxxxxxxxxxxxwm.elasticsearch.aliyuncs.com:9200"]
        index => "elastic_test"
        password => "XXX"
        user => "elastic"
        }
    }
    ```

    |参数|描述|示例值|
    |--|--|---|
    |bootstrap\_servers|消息队列Kafka版的VPC环境接入点。|192.168.xx.xx:9092,192.168.xx.xx:9092,192.168.xx.xx:9092|
    |group\_id|Consumer Group的名称。|elastic\_group|
    |topics|Topic的名称。|elastic\_test|
    |consumer\_threads|消费线程数。建议与Topic的分区数保持一致。|12|
    |decorate\_events|是否包含消息元数据。默认值为false。|true|

    |参数|描述|示例值|
    |--|--|---|
    |hosts|阿里云Elasticsearch服务的访问地址。您可在阿里云Elasticsearch实例的**基本信息**页面获取。|http://es-cn-o40xxxxxxxxxxxxwm.elasticsearch.aliyuncs.com:9200|
    |index|索引的名称。|elastic\_test|
    |password|访问阿里云Elasticsearch服务的密码。您在创建阿里云Elasticsearch实例时设置的密码。|XXX|
    |user|访问阿里云Elasticsearch服务的用户名。您在创建阿里云Elasticsearch实例时设置的用户名。|elastic|

7.  在**管道参数配置**中，输入配置信息，然后单击**保存并部署**。

8.  在**提示**对话框，单击**确认**。


## 步骤七：搜索数据

您可以在Kibana控制台搜索通过管道导入阿里云Elasticsearch的数据，确认数据是否导入成功。

1.  登录[阿里云Elasticsearch控制台](elasticsearch.console.aliyun.com)。

2.  在顶部菜单栏，选择地域。

3.  在**实例列表**页面，单击创建的实例。

4.  在左侧导航栏，单击**可视化控制**。

5.  在**Kibana**区域，单击**进入控制台**。

6.  在Kibana登录页面，输入Username和Password，然后单击**Log in**。

    **说明：**

    -   Username为您创建阿里云Elasticsearch实例时设置的用户名。
    -   Password为您创建阿里云Elasticsearch实例时设置的密码。
7.  在Kibana控制台的左侧导航栏，单击**Dev Tools**图标。

8.  执行以下命令搜索数据。

    ```
    GET /elastic_test/_search
    {}
    ```

    返回结果如下。

    ![作为Input接入](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9240235951/p110331.png)


