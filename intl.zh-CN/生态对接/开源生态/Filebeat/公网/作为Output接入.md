---
keyword: [kafka, filebeat, output, 公网]
---

# 作为Output接入

消息队列Kafka版可以作为Output接入Filebeat。本文说明如何在公网环境下通过Filebeat向消息队列Kafka版发送消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。详情请参见[公网+VPC接入](/intl.zh-CN/快速入门/步骤二：购买和部署实例/公网+VPC接入.md)。
-   下载并安装Filebeat。详情请参见[Download Filebeat](https://www.elastic.co/guide/en/logstash/7.6/installing-logstash.html)。
-   下载并安装JDK 8。详情请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入点

Filebeat通过消息队列Kafka版的接入点与消息队列Kafka版建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**实例详情**。

3.  在**实例详情**页面，选择要作为Output接入Filebeat的实例。

4.  在**基本信息**区域，获取实例的接入点。

    ![endpoint](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p107786.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/intl.zh-CN/产品简介/接入点对比.md)。


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，单击**创建Topic**。

3.  在**创建Topic**页面，输入Topic信息，然后单击**创建**。

    ![createtopic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p107758.png)


## 步骤三：Filebeat发送消息

在安装了Filebeat的机器上启动Filebeat，向创建的Topic发送消息。

1.  执行cd命令切换到Filebeat的安装目录。

2.  执行以下命令下载CA证书文件。

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert
    ```

3.  创建output.conf配置文件。

    1.  执行命令`vim output.conf`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        filebeat.inputs:
        - type: stdin
        
        output.kafka:
          hosts: ["121.XX.XX.XX:9093", "120.XX.XX.XX:9093", "120.XX.XX.XX:9093"]
          username: "alikafka_pre-cn-v641e1d***"
          password: "aeN3WLRoMPRXmAP2jvJuGk84Kuuo***"
        
          topic: 'filebeat_test'
          partition.round_robin:
            reachable_only: false
          ssl.certificate_authorities: ["/root/filebeat/filebeat-7.7.0-linux-x86_64/tasks/vpc_ssl/ca-cert"]
          ssl.verification_mode: none
        
          required_acks: 1
          compression: none
          max_message_bytes: 1000000
        ```

        |参数|描述|示例值|
        |--|--|---|
        |hosts|消息队列Kafka版提供的公网接入点为SSL接入点。|121.XX.XX.XX:9093, 120.XX.XX.XX:9093, 120.XX.XX.XX:9093|
        |username|公网/VPC实例的用户名。|alikafka\_pre-cn-v641e1d\*\*\*|
        |password|公网/VPC实例的密码。|aeN3WLRoMPRXmAP2jvJuGk84Kuuo\*\*\*|
        |topic|Topic的名称。|filebeat\_test|
        |reachable\_only|消息是否只发送到可用的分区。取值：         -   true：如果主分区不可用，输出可能阻塞。
        -   false：即使主分区不可用，输出不被阻塞。
|false|
        |ssl.certificate\_authorities|CA证书所在位置。|/root/filebeat/filebeat-7.7.0-linux-x86\_64/ca-cert|
        |ssl.verification\_mode|认证模式。|none|
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

4.  向创建的Topic发送消息。

    1.  执行`./filebeat -c ./output.yml`。

    2.  输入test，然后按回车键。


## 步骤四：查看Topic分区

查看消息发送到Topic的情况。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，选择作为Output接入Filebeat的实例，找到发送消息的Topic，在其右侧**操作**列单击**分区状态**。

3.  在**分区状态**页面，单击**刷新**。

    发送的消息的分区ID和位点信息如下图所示。

    ![topic_status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p107774.png)


## 步骤五：按位点查询消息

您可以根据发送的消息的分区ID和位点信息查询该消息。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**消息查询**。

2.  在**消息查询**页面，单击**按位点查询**页签。

3.  从**请输入Topic**列表，选择发送了消息的Topic，从**请选择分区**列表，选择发送的消息的分区ID，从**请输入位点**列表，选择发送的消息的位点，然后单击**搜索**。

    ![query_1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p107775.png)

4.  在搜索结果右侧的**操作**列，单击**消息详情**。

    ![query message](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p112483.png)


