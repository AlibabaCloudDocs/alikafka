---
keyword: [kafka, filebeat, input, 公网]
---

# 作为Input接入

消息队列Kafka版可以作为Input接入Filebeat。本文说明如何在公网环境下通过Filebeat从消息队列Kafka版消费消息。

在开始本教程前，请确保您已完成以下操作：

-   购买并部署消息队列Kafka版实例。具体操作，请参见[公网+VPC接入](/intl.zh-CN/快速入门/步骤二：购买和部署实例/公网+VPC接入.md)。
-   下载并安装Filebeat。具体操作，请参见[Download Filebeat](https://www.elastic.co/cn/downloads/beats/filebeat)。
-   下载并安装JDK 8。具体操作，请参见[Download JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)。

## 步骤一：获取接入点

Filebeat通过消息队列Kafka版的接入点与消息队列Kafka版建立连接。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**实例详情**。

3.  在**实例详情**页面，选择要作为Input接入Filebeat的实例。

4.  在**基本信息**区域，获取实例的接入点。

    ![endpoint](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3484976951/p107786.png)

    **说明：** 不同接入点的差异，请参见[接入点对比](/intl.zh-CN/产品简介/接入点对比.md)。


## 步骤二：创建Topic

创建用于存储消息的Topic。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Topic管理**。

2.  在**Topic管理**页面，单击**创建Topic**。

3.  在**创建Topic**对话框，输入Topic信息，然后单击**创建**。

    ![create_topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6103976951/p106204.png)


## 步骤三：发送消息

向创建的Topic发送消息。

1.  在消息队列Kafka版控制台的**Topic管理**页面，找到创建的Topic，在其右侧**操作**列，单击**发送消息**。

2.  在**发送消息**对话框，输入消息信息，然后单击**发送**。

    ![send_msg](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6103976951/p106203.png)


## 步骤四：创建Consumer Group

创建Filebeat所属的Consumer Group。

1.  在消息队列Kafka版控制台的左侧导航栏，单击**Consumer Group管理**。

2.  在**Consumer Group管理**页面，单击**创建Consumer Group**。

3.  在**创建Consumer Group**对话框，输入Consumer Group信息，然后单击**创建**。

    ![create_cg](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6103976951/p106205.png)


## 步骤五：Filebeat消费消息

在安装了Filebeat的机器上启动Filebeat，从创建的Topic中消费消息。

1.  执行cd命令切换到Filebeat的安装目录。

2.  执行以下命令下载CA证书文件。

    ```
    wget https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert
    ```

3.  创建input.yml配置文件。

    1.  执行命令`vim input.yml`创建空的配置文件。

    2.  按i键进入插入模式。

    3.  输入以下内容。

        ```
        filebeat.inputs:
        - type: kafka
          hosts:
            - 121.XX.XX.XX:9093
            - 120.XX.XX.XX:9093
            - 120.XX.XX.XX:9093
          username: "alikafka_pre-cn-v641e1dt***"
          password: "aeN3WLRoMPRXmAP2jvJuGk84Kuuo***"
          topics: ["filebeat_test"]
          group_id: "filebeat_group"
          ssl.certificate_authorities: ["/root/filebeat/filebeat-7.7.0-linux-x86_64/ca-cert"]
          ssl.verification_mode: none
        
        output.console:
          pretty: true
        ```

        |参数|描述|示例值|
        |--|--|---|
        |hosts|消息队列Kafka版提供的公网接入点为SSL接入点。|        ```
- 121.XX.XX.XX:9093
- 120.XX.XX.XX:9093
- 120.XX.XX.XX:9093
        ``` |
        |username|公网/VPC实例的用户名。|alikafka\_pre-cn-v641e1d\*\*\*|
        |password|公网/VPC实例的密码。|aeN3WLRoMPRXmAP2jvJuGk84Kuuo\*\*\*|
        |topics|Topic的名称。|filebeat\_test|
        |group\_id|Consumer Group的名称。|filebeat\_group|
        |ssl.certificate\_authorities|CA证书所在位置。|/root/filebeat/filebeat-7.7.0-linux-x86\_64/ca-cert|
        |ssl.verification\_mode|认证模式。|none|

        更多参数设置，请参见[Kafka input plugin](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-kafka.html)。

    4.  按Esc键回到命令行模式。

    5.  按：键进入底行模式，输入wq，然后按回车键保存文件并退出。

4.  执行以下命令消费消息。

    ```
    ./filebeat -c ./input.yml
    ```

    ![result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6103976951/p107686.png)


