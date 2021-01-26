---
keyword: [kafka, logstash, output, 公网]
---

# 作为Output接入

消息队列Kafka版可以作为Output接入Logstash。本文说明如何在公网环境下通过Logstash向消息队列Kafka版发送消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。具体操作，请参见[公网+VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/公网+VPC接入.md)。
-   下载并安装Logstash。具体操作，请参见[Download Logstash](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。具体操作，请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入点

Logstash通过消息队列Kafka版的接入点与消息队列Kafka版建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在左侧导航栏，单击**实例列表**。

3.  在**实例列表**页面，单击要作为Output接入Logstash的实例的名称。

4.  在**实例详情**页面的**基本信息**区域，获取实例的接入点。

    ![endpointzh](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1836461161/p232431.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

5.  在**安全配置**区域，获取实例的用户名和密码。


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在左侧导航栏，单击**实例列表**。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Topic管理**。

5.  在**Topic管理**页面，单击**创建Topic**。

6.  在**创建Topic**对话框，输入Topic信息，然后单击**创建**。

    ![create topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1451561161/p232533.png)


## 步骤三：Logstash发送消息

在安装了Logstash的机器上启动Logstash，向创建的Topic发送消息。

1.  执行cd命令切换到logstash的bin目录。

2.  执行以下命令下载kafka.client.truststore.jks证书文件。

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-log-stash-demo/vpc-ssl/kafka.client.truststore.jks
    ```

3.  创建jaas.conf配置文件。

    1.  执行命令`vim jaas.conf`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        KafkaClient {
          org.apache.kafka.common.security.plain.PlainLoginModule required
          username="XXX"
          password="XXX";
        };
        ```

        |参数|描述|示例值|
        |--|--|---|
        |username|公网/VPC实例的用户名。|alikafka\_pre-cn-v0h1\*\*\*|
        |password|公网/VPC实例的密码。|GQiSmqbQVe3b9hdKLDcIlkrBK6\*\*\*|

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

4.  创建output.conf配置文件。

    1.  执行命令`vim output.conf`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        input {
            stdin{}
        }
        
        output {
           kafka {
                bootstrap_servers => "121.40.XXX.XXX:9093,120.26.XXX.XXX:9093,120.26.XXX.XXX:9093"
                topic_id => "logstash_test"
                security_protocol => "SASL_SSL"
                sasl_mechanism => "PLAIN"
                jaas_path => "/home/logstash-7.6.2/bin/jaas.conf"
                ssl_truststore_password => "KafkaOnsClient"
                ssl_truststore_location => "/home/logstash-7.6.2/bin/kafka.client.truststore.jks"
                ssl_endpoint_identification_algorithm => ""
            }
        }
        ```

        |参数|描述|示例值|
        |--|--|---|
        |bootstrap\_servers|消息队列Kafka版提供的公网接入点为SSL接入点。|121.XX.XX.XX:9093,120.XX.XX.XX:9093,120.XX.XX.XX:9093|
        |topic\_id|Topic的名称。|logstash\_test|
        |security\_protocol|安全协议。默认为SASL\_SSL，无需修改。|SASL\_SSL|
        |sasl\_mechanism|安全认证机制。默认为PLAIN，无需修改。|PLAIN|
        |jaas\_path|jaas.conf配置文件位置。|/home/logstash-7.6.2/bin/jaas.conf|
        |ssl\_truststore\_password|kafka.client.truststore.jks证书密码。默认值为KafkaOnsClient，无需修改。|KafkaOnsClient|
        |ssl\_truststore\_location|kafka.client.truststore.jks证书位置。|/home/logstash-7.6.2/bin/kafka.client.truststore.jks|
        |ssl\_endpoint\_identification\_algorithm|SSL接入点辨识算法。6.x及以上版本Logstash需要加上该参数。|空值|

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

5.  向创建的Topic发送消息。

    1.  执行`./logstash -f output.conf`。

    2.  输入test，然后按回车键。

        ![output_result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8203976951/p106190.png)


## 步骤四：查看Topic分区

查看消息发送到Topic的情况。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在左侧导航栏，单击**实例列表**。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Topic管理**。

5.  在**Topic管理**页面，找到发送消息的Topic，在其右侧**操作**列，单击**分区状态**。

6.  在**分区状态**对话框，单击![update](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1836461161/p232497.png)刷新。

    发送的消息的分区ID和位点信息如下图所示。

    ![topic_atatus](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3050561161/p232509.png)


## 步骤五：按位点查询消息

您可以根据发送的消息的分区ID和位点信息查询该消息。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在左侧导航栏，单击**实例列表**。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**消息查询**。

5.  在**消息查询**页面，单击**按位点查询**页签。

6.  从**请输入Topic**列表，选择发送了消息的Topic，从**请选择分区**列表，选择发送的消息的分区ID，从**请输入位点**列表，选择发送的消息的位点，然后单击**查询**。

7.  在查询结果右侧，可以查看消息详情。


## 更多信息

更多参数设置，请参见[Kafka output plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-kafka.html)。

