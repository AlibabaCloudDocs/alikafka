---
keyword: [kafka, logstash, input, 公网]
---

# 作为Input接入

消息队列Kafka版可以作为Input接入Logstash。本文说明如何在公网环境下通过Logstash从消息队列Kafka版消费消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。具体操作，请参见[公网+VPC接入](/intl.zh-CN/快速入门/步骤二：购买和部署实例/公网+VPC接入.md)。
-   下载并安装Logstash。具体操作，请参见[Download Logstash](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。具体操作，请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入信息

Logstash通过消息队列Kafka版的接入点与消息队列Kafka版建立连接，并通过用户名及密码进行校验。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击作为Input接入Logstash的实例名称。


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
            1.  单击**是**，在**分区 ID**文本框中输入分区的ID，例如0。如果您需查询分区的ID，请参见[查看分区状态](/intl.zh-CN/用户指南/Topic/查看分区状态.md)。
            2.  单击**否**，不指定分区。
        4.  根据界面提示信息，通过SDK订阅消息，或者执行Docker命令订阅消息。
    -   **发送方式**选择**Docker**，运行Docker容器。
        1.  执行**运行 Docker 容器生产示例消息**区域的Docker命令，发送消息。
        2.  执行**发送后如何消费消息？**区域的Docker命令，订阅消息。
    -   **发送方式**选择**SDK**，根据您的业务需求，选择需要的语言或者框架的SDK以及接入方式，通过SDK体验消息收发。

## 步骤四：创建Consumer Group

创建Logstash所属的Consumer Group。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Group 管理**。

5.  在**Group 管理**页面，单击**创建 Group**。

6.  在**创建 Group**面板的**Group ID**文本框输入Group的名称，在**描述**文本框简要描述Group，并给Group添加标签，单击**确定**。

    创建完成后，在**Group 管理**页面的列表中显示已创建的Group。


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

