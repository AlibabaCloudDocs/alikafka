# 在Knative上实现Kafka消息推送

Knative已支持Kafka事件源，您可将Knative与消息队列Kafka版对接，在Knative上实现Kafka消息推送。

-   [一键部署Knative](/cn.zh-CN/Kubernetes集群用户指南/Knative管理/Knative组件管理/一键部署Knative.md)
-   [购买并部署消息队列Kafka版实例](/cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC接入.md)

    **说明：**

    -   消息队列Kafka版实例和Knative必须处于同一VPC内。
    -   消息队列Kafka版实例的版本必须为2.0.0或以上。
-   [创建Topic](/cn.zh-CN/快速入门/步骤三：创建资源.md)
-   [创建Consumer Group](/cn.zh-CN/快速入门/步骤三：创建资源.md)

Knative是一款基于Kubernetes的Serverless框架，其目标是制定云原生、跨平台的Serverless编排标准。Knative主要包括：

-   Serving：服务系统，用于配置应用的路由、升级策略、自动扩缩容等。
-   Eventing：事件系统，用于自动完成事件的绑定和触发。

要让Eventing（事件系统）正常运行，就必须在Knative集群中实现Channel（内部事件存储层），目前支持的Channel实现方式包括Kafka、NATS。本文以消息队列Kafka版为例介绍如何实现Channel。

## 操作流程

在Knative上实现Kafka消息推送的操作流程如下图所示。

![dg_task_flow](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1398900161/p75834.jpg)

## 部署Kafka组件

1.  登录[容器服务控制台](https://cs.console.aliyun.com)。

2.  在左侧导航栏，单击**集群**。

3.  在**集群列表**页面，单击要部署Kafka组件的集群的集群名称。

4.  在**左侧导航栏**，选择**应用** \> **Knative**。

5.  在**组件管理**页签下的**add-on组件**区域，找到Kafka，在其右侧**操作**列，单击**部署**。

6.  在**部署Kafka**对话框，单击**确定**。

    部署完成后，**Kafka**右侧的**状态**显示**已部署**。

    ![Knative_2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1398900161/p98497.png)


## 创建event-display服务

1.  在**Knative组件管理**页面，单击**服务管理**页签。

2.  在**Knative服务管理**页面，单击**使用模板创建**。

3.  在**使用模板创建**页面：

    1.  从**集群**列表，选择已部署Knative组件的集群。

    2.  从**命名空间**列表，选择**default**。

    3.  从**示例模板**列表，选择**自定义**。

    4.  在**模板**区域，输入模板信息。

        ```
        apiVersion: serving.knative.dev/v1
        kind: Service
        metadata:
          name: event-display
        spec:
          template:
            metadata:
              annotations:
                autoscaling.knative.dev/minScale: "1"
            spec:
              containers:
              - image: registry.cn-hangzhou.aliyuncs.com/knative-sample/eventing-sources-cmd-event_display:bf45b3eb1e7fc4cb63d6a5a6416cf696295484a7662e0cf9ccdf5c080542c21d
        ```

    5.  单击**创建**。

    6.  单击**返回**。

        创建完成后，**event-display**右侧的**状态**显示**成功**。

        ![pg_event_display](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1398900161/p98505.png)


## 创建kafka-source服务

1.  [通过kubectl连接Kubernetes集群](/cn.zh-CN/Kubernetes集群用户指南/集群管理/连接集群/通过kubectl连接Kubernetes集群.md)。

2.  创建KafkaSource服务的配置文件kafka-source.yaml。

    ```
    apiVersion: sources.eventing.knative.dev/v1alpha1
    kind: KafkaSource
    metadata:
      name: kafka-source
    spec:
      consumerGroup: demo-topic
      # Broker URL. Replace this with the URLs for your kafka cluster,
      # which is in the format of my-cluster-kafka-bootstrap.my-kafka-namespace:9092.
      bootstrapServers: 192.168.X.XXX:9092,192.168.X.XXX:9092,192.168.X.XXX:9092
      topics: demo
      sink:
        apiVersion: serving.knative.dev/v1alpha1
        kind: Service
        name: event-display
    ```

    |参数|说明|示例值|
    |--|--|---|
    |consumerGroup|创建的Consumer Group的名称。|demo-consumer|
    |bootstrapServers|消息队列Kafka版实例的默认接入点。|192.168.X.XXX:9092,192.168.X.XXX:9092,192.168.X.XXX:9092|
    |topics|创建的Topic的名称。|demo-topic|

3.  执行以下命令创建KafkaSource服务。

    ```
    kubectl apply -f kafka-source.yaml
    ```

    返回示例如下。

    ```
    cb kafkasource.sources.knative.dev/kafka-source created
    ```


## 发送消息

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4e.11153940.0.0.473e500dpMSQGl#/TopicManagement?regionId=cn-hangzhou&instanceId=alikafka_pre-cn-4591fbkd400a)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，找到目标实例，在其右侧**操作**列，单击**详情**。

5.  在左侧导航栏，单击**Topic管理**。

6.  在**Topic管理**页面，找到目标Topic，在其右侧**操作**列，选择**![more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2398900161/p211711.png)** \> **发送消息**。

7.  在**发送消息**面板：

    1.  在**Message Key**文本框，输入demo。

    2.  在**Message Value**文本框，输入\{"key": "test"\}。

        **说明：** Message Value必须为JSON格式。

        ![send_message](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2398900161/p211695.png)


## 结果验证

发送消息后，通过`kubectl logs`命令查看event-display服务的日志，确认event-display服务已接收到消息队列Kafka版发送的消息。

