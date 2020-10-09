---
keyword: [kafka, elasticsearch, vpc]
---

# 将消息队列Kafka版接入阿里云Elasticsearch

随着时间的积累，消息队列Kafka版中的日志数据会越来越多。当您需要查看并分析庞杂的日志数据时，可通过阿里云Logstash将消息队列Kafka版中的日志数据导入阿里云Elasticsearch，然后通过Kibana进行可视化展示与分析。本文说明如何进行操作。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。详情请参见[VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)。
-   创建阿里云Elasticsearch实例。详情请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

    **说明：** 请注意保存创建阿里云Elasticsearch实例时设置的用户名及密码。该用户名及密码将用于[步骤五：创建索引](#section_awp_hjm_4to)、[步骤六：创建管道](#section_x33_eux_vin)和[步骤七：搜索数据](#section_0lt_6hh_dfu)。

-   创建阿里云Logstash实例。详情请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash实例/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

通过阿里云Logstash将数据从消息队列Kafka版导入阿里云Elasticsearch的过程如下图所示。

![elasticsearch](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p111467.png)

-   消息队列Kafka版

    消息队列Kafka版是阿里云提供的分布式、高吞吐、可扩展的消息队列服务。消息队列Kafka版广泛用于日志收集、监控数据聚合、流式数据处理、在线和离线分析等大数据领域，已成为大数据生态中不可或缺的部分。详情请参见[什么是消息队列Kafka版](/cn.zh-CN/产品简介/什么是消息队列Kafka版？.md)。

-   阿里云Elasticsearch

    Elasticsearch简称ES，是一个基于Lucene的实时分布式的搜索与分析引擎，是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。它提供了一个分布式服务，可以使您快速的近乎于准实时的存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术。阿里云Elasticsearch支持5.5.3、6.3.2、6.7.0、6.8.0和7.4.0版本，并提供了商业插件X-Pack服务，致力于数据分析、数据搜索等场景服务。在开源Elasticsearch的基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。详情请参见[什么是阿里云Elasticsearch](/cn.zh-CN/产品简介/什么是阿里云Elasticsearch.md)。

-   阿里云Logstash

    阿里云Logstash作为服务器端的数据处理管道，提供了100%兼容开源的Logstash功能。Logstash能够动态地从多个来源采集数据、转换数据，并且将数据存储到所选择的位置。通过输入、过滤和输出插件，Logstash可以对任何类型的事件加工和转换。详情请参见[什么是阿里云Logstash](/cn.zh-CN/Logstash实例/什么是阿里云Logstash.md)。


## 步骤一：获取VPC环境接入点

阿里云Logstash通过消息队列Kafka版的接入点与消息队列Kafka版在VPC环境下建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在顶部菜单栏，选择地域，例如华东1（杭州）。

3.  在左侧导航栏，单击**实例详情**。

4.  在**实例详情**页面，选择要将数据导入阿里云Elasticsearch的实例。

5.  在**基本信息**区域，获取实例的VPC环境接入点。

    ![endpoint](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p111363.png)

    消息队列Kafka版支持以下VPC环境接入点：

    -   默认接入点：端口号为9092。
    -   SASL接入点：端口号为9094。如需使用SASL接入点，请开启ACL。您可以[提交工单](https://selfservice.console.aliyun.com/#/ticket/category/alikafka)申请开启ACL。
    详情请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，单击**创建Topic**。

3.  在**创建Topic**页面，输入Topic信息，然后单击**创建**。

    ![create_topic](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p109846.png)


## 步骤三：发送消息

向创建的Topic发送消息。

1.  在消息队列Kafka版控制台的**Topic管理**页面，找到创建的Topic，在其右侧**操作**列，单击**发送消息**。

2.  在**发送消息**对话框，输入消息信息，然后单击**发送**。

    ![send_msg](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p109847.png)


## 步骤四：创建Consumer Group

创建阿里云Elasticsearch所属的Consumer Group。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Consumer Group管理**。

2.  在**Consumer Group管理**页面，单击**创建Consumer Group**。

3.  在**创建Consumer Group**页面，输入Consumer Group信息，然后单击**创建**。

    ![create_cg](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p109848.png)


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

    ![作为Input接入](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9240235951/p110331.png)


