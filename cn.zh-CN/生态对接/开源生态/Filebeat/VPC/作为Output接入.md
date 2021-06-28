---
keyword: [, , ]
---

# 作为Output接入

消息队列Kafka版可以作为Output接入Filebeat。本文说明如何在VPC环境下通过Filebeat向消息队列Kafka版发送消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。更多信息，请参见[VPC接入](/cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)。
-   下载并安装Filebeat。更多信息，请参见[Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。更多信息，请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入点

Filebeat通过消息队列Kafka版的接入点与消息队列Kafka版建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击作为Output接入Filebeat的实例名称。

4.  在**实例详情**页面的**接入点信息**区域，获取实例的接入点。在**配置信息**区域，获取**用户名**与**密码**。

    ![endpoint](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7733254261/p111363.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。


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
    |**存储引擎**|Topic消息的存储引擎。消息队列Kafka版支持以下两种存储引擎。

    -   **云存储**：底层接入阿里云云盘，具有低时延、高性能、持久性、高可靠等特点，采用分布式3副本机制。实例的**规格类型**为**标准版（高写版）**时，存储引擎只能为**云存储**。
    -   **Local 存储**：使用原生Kafka的ISR复制算法，采用分布式3副本机制。
|**云存储**|
    |**消息类型**|Topic消息的类型。    -   **普通消息**：默认情况下，保证相同Key的消息分布在同一个分区中，且分区内消息按照发送顺序存储。集群中出现机器宕机时，可能会造成消息乱序。当**存储引擎**选择**云存储**时，默认选择**普通消息**。
    -   **分区顺序消息**：默认情况下，保证相同Key的消息分布在同一个分区中，且分区内消息按照发送顺序存储。集群中出现机器宕机时，仍然保证分区内按照发送顺序存储。但是会出现部分分区发送消息失败，等到分区恢复后即可恢复正常。当**存储引擎**选择**Local 存储**时，默认选择**分区顺序消息**。
|**普通消息**|
    |**日志清理策略**|Topic日志的清理策略。当**存储引擎**选择**Local 存储**时，需要配置**日志清理策略**。

消息队列Kafka版支持以下两种日志清理策略。

    -   **Delete**：默认的消息清理策略。在磁盘容量充足的情况下，保留在最长保留时间范围内的消息；在磁盘容量不足时（一般磁盘使用率超过85%视为不足），将提前删除旧消息，以保证服务可用性。
    -   **Compact**：使用[Kafka Log Compaction日志清理策略](https://kafka.apache.org/documentation/?spm=a2c4g.11186623.2.15.1cde7bc3c8pZkD#compaction)。Log Compaction清理策略保证相同Key的消息，最新的value值一定会被保留。主要适用于系统宕机后恢复状态，系统重启后重新加载缓存等场景。例如，在使用Kafka Connect或Confluent Schema Registry时，需要使用Kafka Compact Topic存储系统状态信息或配置信息。

**说明：** Compact Topic一般只用在某些生态组件中，例如Kafka Connect或Confluent Schema Registry，其他情况的消息收发请勿为Topic设置该属性。具体信息，请参见[消息队列Kafka版Demo库](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master)。

|**Compact**|
    |**标签**|Topic的标签。|demo|

    创建完成后，在**Topic 管理**页面的列表中显示已创建的Topic。


## 步骤三：Filebeat发送消息

在安装了Filebeat的机器上启动Filebeat，向创建的Topic发送消息。

1.  执行cd命令切换到Filebeat的安装目录。

2.  创建output.conf配置文件。

    1.  执行命令`vim output.conf`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        filebeat.inputs:
        - type: stdin
        
        output.kafka:
          hosts: ["192.XX.XX.XX:9092", "192.XX.XX.XX:9092", "192.XX.XX.XX:9092"]
        
          topic: 'filebeat_test'
        
          required_acks: 1
          compression: none
          max_message_bytes: 1000000
        ```

        |参数|描述|示例值|
        |--|--|---|
        |hosts|消息队列Kafka版提供以下VPC接入点：         -   默认接入点
        -   SASL接入点
|192.168.XXX.XXX:9092,192.168.XXX.XXX:9092,192.168.XXX.XXX:9092|
        |topic|Topic的名称。|filebeat\_test|
        |required\_acks|ACK可靠性。取值：         -   0：无响应
        -   1：等待本地提交
        -   -1：等待所有副本提交
默认值为1。|1|
        |compression|数据压缩编译码器。默认值为gzip。取值：         -   none：无
        -   snappy：用来压缩和解压缩的C++开发包
        -   lz4：着重于压缩和解压缩速度的无损数据压缩算法
        -   gzip：GNU自由软件的文件压缩程序
|none|
        |max\_message\_bytes|最大消息大小。单位为字节。默认值为1000000。该值应小于您配置的消息队列Kafka版最大消息大小。|1000000|

        更多参数说明，请参见[Kafka output plugin](https://www.elastic.co/guide/en/beats/filebeat/current/kafka-output.html)。

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

3.  向创建的Topic发送消息。

    1.  执行`./filebeat -c ./output.yml`。

    2.  输入test，然后按回车键。


## 步骤四：查看Topic分区

查看消息发送到Topic的情况。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**Topic 管理**。

5.  在**Topic 管理**页面，找到目标Topic，在其**操作**列中，选择**更多** \> **分区状态**。

    |参数|说明|
    |--|--|
    |分区ID|该Topic分区的ID号。|
    |最小位点|该Topic在当前分区下的最小消费位点。|
    |最大位点|该Topic在当前分区下的最大消费位点。|
    |最近更新时间|本分区中最近一条消息的存储时间。|

    ![分区状态信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3855612261/p278003.png)


## 步骤五：按位点查询消息

您可以根据发送的消息的分区ID和位点信息查询该消息。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在**概览**页面的**资源分布**区域，选择地域。

3.  在**实例列表**页面，单击目标实例名称。

4.  在左侧导航栏，单击**消息查询**。

5.  在**消息查询**页面的**查询方式**列表中，选择**按位点查询**。

6.  在**Topic**列表中，选择消息所属Topic名称；在**分区**列表中，选择消息所属的分区；在**起始位点**文本框，输入消息所在分区的位点，然后单击**查询**。

    展示该查询位点及以后连续的消息。例如，指定的分区和位点都为“5”，那么返回的结果从位点“5”开始。

    |参数|描述|
    |--|--|
    |**分区**|消息的Topic分区。|
    |**位点**|消息的所在的位点。|
    |**Key**|消息的键（已强制转化为String类型）。|
    |**Value**|消息的值（已强制转化为String类型），即消息的具体内容。|
    |**消息创建时间**|发送消息时，客户端自带的或是您指定的`ProducerRecord`中的消息创建时间。**说明：**

    -   如果配置了该字段，则按配置值显示。
    -   如果未配置该字段，则默认取消息发送时的系统时间。
    -   如果显示值为1970/x/x x:x:x，则说明发送时间配置为0或其他有误的值。
    -   0.9及以前版本的消息队列Kafka版客户端不支持配置该时间。 |
    |**操作**|    -   单击**下载 Key**：下载消息的键值。
    -   单击**下载 Value**：下载消息的具体内容。
**说明：**

    -   查询到的每条消息在控制台上最多显示1 KB的内容，超过1 KB的部分将自动截断。如需查看完整的消息内容，请下载相应的消息。
    -   仅专业版支持下载消息。
    -   下载的消息最大为10 MB。如果消息超过10 MB，则只下载10 MB的内容。 |


