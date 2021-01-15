---
keyword: [kafka, logstash, input, 公网]
---

# 作为Input接入

消息队列Kafka版可以作为Input接入Logstash。本文说明如何在公网环境下通过Logstash从消息队列Kafka版消费消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。详情请参见[公网+VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/公网+VPC接入.md)。
-   下载并安装Logstash。详情请参见[Download Logstash](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。详情请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入信息

Logstash通过消息队列Kafka版的接入点与消息队列Kafka版建立连接，并通过用户名及密码进行校验。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**实例详情**。

3.  在**实例详情**页面，选择要作为Input接入Logstash的实例。

4.  在**基本信息**区域，获取实例的接入点。

    ![step4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p110459.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

5.  在**安全配置**区域，获取实例的用户名和密码。

    ![configure](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p110458.png)


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，单击**创建Topic**。

3.  在**创建Topic**页面，输入Topic信息，然后单击**创建**。

    ![logstash_2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p103888.png)


## 步骤三：发送消息

向创建的Topic发送消息。

1.  在消息队列Kafka版控制台的**Topic管理**页面，找到创建的Topic，在其右侧**操作**列，单击**发送消息**。

2.  在**发送消息**对话框，输入消息信息，然后单击**发送**。

    ![logstash_4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p103896.png)


## 步骤四：创建Consumer Group

创建Logstash所属的Consumer Group。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Consumer Group管理**。

2.  在**Consumer Group管理**页面，单击**创建Consumer Group**。

3.  在**创建Consumer Group**页面，输入Consumer Group信息，然后单击**创建**。

    ![logstash_3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p103892.png)


## 步骤五：Logstash消费消息

在安装了Logstash的机器上启动Logstash，从创建的Topic中消费消息。

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

4.  创建input.conf配置文件。

    1.  执行命令`vim input.conf`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        input {
            kafka {
                bootstrap_servers => "121.XX.XX.XX:9093,120.XX.XX.XX:9093,120.XX.XX.XX:9093"
                topics => ["logstash_test"]
        
                security_protocol => "SASL_SSL"
                sasl_mechanism => "PLAIN"
        
                jaas_path => "/home/logstash-7.6.2/bin/jaas.conf"
        
                ssl_truststore_password => "KafkaOnsClient"
                ssl_truststore_location => "/home/logstash-7.6.2/bin/kafka.client.truststore.jks"
        
                ssl_endpoint_identification_algorithm => ""
        
                group_id => "logstash_group"
                consumer_threads => 3
                auto_offset_reset => "earliest"
            }
        }
        
        output {
            stdout {
                codec => rubydebug
            }
        }
        ```

        |参数|描述|示例值|
        |--|--|---|
        |bootstrap\_servers|消息队列Kafka版提供的公网接入点为SSL接入点。|121.XX.XX.XX:9093,120.XX.XX.XX:9093,120.XX.XX.XX:9093|
        |topics|Topic的名称。|logstash\_test|
        |security\_protocol|安全协议。默认为SASL\_SSL，无需修改。|SASL\_SSL|
        |sasl\_mechanism|安全认证机制。默认为PLAIN，无需修改。|PLAIN|
        |jaas\_path|jaas.conf配置文件位置。|/home/logstash-7.6.2/bin/jaas.conf|
        |ssl\_truststore\_password|kafka.client.truststore.jks证书密码。默认值为KafkaOnsClient，无需修改。|KafkaOnsClient|
        |ssl\_truststore\_location|kafka.client.truststore.jks证书位置。|/home/logstash-7.6.2/bin/kafka.client.truststore.jks|
        |ssl\_endpoint\_identification\_algorithm|6.x及以上版本Logstash需要加上该参数。|空值|
        |group\_id|Cosumer Group的名称。|logstash\_group|
        |consumer\_threads|消费线程数。建议与Topic的分区数保持一致。|3|
        |auto\_offset\_reset|重置偏移量。取值：         -   earliest：读取最早的消息。
        -   latest：读取最新的消息。
|earliest|

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

5.  执行以下命令消费消息。

    ```
    ./logstash -f input.conf
    ```

    返回结果如下。

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9284976951/p105037.png)


## 更多信息

更多参数设置，请参见[Kafka input plugin](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-kafka.html)。

