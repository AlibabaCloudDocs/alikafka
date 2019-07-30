# VPC 接入 {#concept_99954_zh .concept}

本文介绍如何使用 VPC 网络接入消息队列 Kafka 服务。

## 前提条件 {#section_oj7_x9q_8s1 .section}

您已搭建了自己的阿里云 VPC 网络。

## 操作步骤 {#section_9ik_8t1_jbn .section}

test

1.  **购买实例**

    1.  登录[消息队列 Kafka 控制台](http://kafka.console.aliyun.com/)。

    2.  在控制台的顶部导航栏，选择您将要购买和部署实例的地域（下图举例为**华东1（杭州）**，您可以根据需求进行切换）。

        **说明：** 消息队列 Kafka 实例不支持跨地域部署，即您需在购买实例的地域进行部署。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445825953129_zh-CN.png)

    3.  在消息队列 Kafka 控制台，单击左侧导航栏中的**概览**。

    4.  在实例列表页，单击**购买新实例**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445825953131_zh-CN.png)

    5.  在实例购买页，根据自身业务需求选择相应的配置。注意，在**实例类型**一栏，选择**VPC实例**类型。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445825953132_zh-CN.png)

    6.  单击页面右侧的**立即购买**，按照提示完成购买流程。

2.  **获取 VPC 信息**

    1.  登录 [VPC 管理控制台](https://vpcnext.console.aliyun.com/)。在左侧导航栏，单击**交换机**。

    2.  在交换机页面，查看以下信息：

        -   VSwitch ID

        -   VPC ID

        -   可用区

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445825953133_zh-CN.png)

        **说明：** 请根据该页面的可用区（A～G）在消息队列 Kafka 控制台中选择对应的可用区（A～G）。例如，某 VPC 交换机（VSwitch）显示在**可用区E**，那么在消息队列 Kafka 控制台中就相应地选择**可用区E**。

3.  **部署实例**

    1.  在消息队列 Kafka 控制台，单击左侧导航栏的**概览**，查看已购买的实例。

    2.  选择处于**待部署**状态的实例，单击右侧的**部署**，然后根据页面提示填写在步骤二中已获取的 VPC 信息。

        完成后，实例会进入**部署中**状态。实例部署预计需要 10 分钟 ~ 30 分钟。

        **说明：** **多接入点**和**重新设置用户名密码**这两项功能，不建议在纯 VPC 场景中使用，请保持默认**否**。

    3.  刷新控制台页面。实例状态显示**服务中**，即表示已部署成功。

4.  **查看实例详情**

    1.  实例部署成功后，单击该实例右侧的**详情**，查看实例详情。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445825953135_zh-CN.png)

        **说明：** 您也可在左侧导航栏单击**实例详情**，然后直接在页面上方选择要查看的实例，单击查看其详情。

    2.  在实例详情页，查看该实例的**默认接入点**。从 VPC 内接入消息队列 Kafka 服务，需使用**默认接入点**接入。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998822/156445826053136_zh-CN.png)


