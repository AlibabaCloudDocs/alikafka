---
keyword: [go, VPC, 收发消息, PLAIN]
---

# SASL接入点PLAIN机制收发消息

本文介绍如何在VPC环境下通过SASL接入点接入消息队列Kafka版并使用PLAIN机制收发消息。

您已安装Go。更多信息，请参见[安装Go](https://golang.org/dl/)。

**说明：** 该kafka-confluent-go-demo不支持Windows系统。

## 准备配置

1.  访问[aliware-kafka-demos](https://github.com/AliwareMQ/aliware-kafka-demos)，单击![code](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7278540261/p271734.png)图标，然后在下拉框选择**Download ZIP**，下载Demo包并解压。

2.  在解压的Demo包中，找到kafka-confluent-go-demo文件夹，将此文件夹上传在Linux系统的/home路径下。

3.  登录Linux系统，进入/home/kafka-confluent-go-demo路径，修改配置文件conf/kafka.json。

    ```
    {
      "topic": "XXX",
      "group.id": "XXX",
      "bootstrap.servers" : "XXX:XX,XXX:XX,XXX:XX",
      "security.protocol" : "sasl_plaintext",
      "sasl.mechanism" : "PLAIN",
      "sasl.username" : "XXX",
      "sasl.password" : "XXX"
    }
    ```

    |参数|描述|是否必须|
    |--|--|----|
    |topic|实例的Topic名称。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Topic管理**页面获取。|是|
    |group.id|实例的Consumer Group。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Consumer Group管理**页面获取。|否|
    |bootstrap.servers|SASL接入点的IP地址以及端口。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**实例详情**页面的**基本信息**区域获取。|是|
    |security.protocol|SASL用户认证协议，默认为plaintext，需配置为sasl\_plaintext。|是|
    |sasl.mechanism|消息收发的机制，默认为PLAIN。|是|
    |sasl.username|SASL用户名。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)**实例详情**页面的**SASL用户**页签获取。**说明：** 如果实例已开启ACL，请确保要接入的SASL用户为PLAIN类型且已授权收发消息的权限。具体操作，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。

|是|
    |sasl.password|SASL用户密码。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)**实例详情**页面的**SASL用户**页签获取。**说明：** 如果实例已开启ACL，请确保要接入的SASL用户为PLAIN类型且已授权收发消息的权限。具体操作，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。

|是|


## 发送消息

1.  执行以下命令发送消息。

    ```
    go run -mod=vendor producer/producer.go
    ```


## 订阅消息

1.  执行以下命令消费消息。

    ```
    go run -mod=vendor consumer/consumer.go
    ```


