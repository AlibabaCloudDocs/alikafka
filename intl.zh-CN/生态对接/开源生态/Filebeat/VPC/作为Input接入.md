---
keyword: [kafka, filebeat, input, vpc]
---

# 作为Input接入

消息队列Kafka版可以作为Input接入Filebeat。本文说明如何在VPC环境下通过Filebeat从消息队列Kafka版消费消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。具体操作，请参见[VPC接入](/intl.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)。
-   下载并安装Filebeat。具体操作，请参见[Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。具体操作，请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入点

Filebeat通过消息队列Kafka版的接入点与消息队列Kafka版建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击要作为Input接入Filebeat的实例名称。

4.  在**实例详情**页面的**接入点信息**页签，获取实例的接入点。

    ![endpoint](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2804172261/p111363.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/intl.zh-CN/产品简介/接入点对比.md)。


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

创建Filebeat所属的Consumer Group。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Group 管理**。

5.  在**Group 管理**页面，单击**创建 Group**。

6.  在**创建 Group**面板的**Group ID**文本框输入Group的名称，在**描述**文本框简要描述Group，并给Group添加标签，单击**确定**。

    创建完成后，在**Group 管理**页面的列表中显示已创建的Group。


## 步骤五：Filebeat消费消息

在安装了Filebeat的机器上启动Filebeat，从创建的Topic中消费消息。

1.  执行cd命令切换到Filebeat的安装目录。

2.  创建input.yml配置文件。

    1.  执行命令`vim input.yml`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        filebeat.inputs:
        - type: kafka
          hosts:
            - 192.168.XX.XX:9092
            - 192.168.XX.XX:9092
            - 192.168.XX.XX:9092
          topics: ["filebeat_test"]
          group_id: "filebeat_group"
        
        output.console:
          pretty: true
        ```

        |参数|描述|示例值|
        |--|--|---|
        |type|Filebeat的Input类型。|kafka|
        |hosts|消息队列Kafka版提供以下VPC接入点：         -   默认接入点
        -   SASL接入点
|        ```
- 192.168.XX.XX:9092
- 192.168.XX.XX:9092
- 192.168.XX.XX:9092
        ``` |
        |topics|Topic的名称。|filebeat\_test|
        |group\_id|Consumer Group的名称。|filebeat\_group|

        更多参数说明，请参见[Kafka input plugin](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html)。

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

3.  执行以下命令消费消息。

    ```
    ./filebeat -c ./input.yml
    ```

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9684125951/p106207.png)


