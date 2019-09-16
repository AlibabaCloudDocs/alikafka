# 公网 + VPC 接入 {#concept_99956_zh .concept}

本文介绍如何同时使用 VPC 网络和公网网络接入消息队列 for Apache Kafka 服务。在此场景下，您只需购买公网实例，采用公网和 VPC 的方式部署即可。

## 前提条件 {#section_ijc_ilt_0tb .section}

您已搭建了自己的阿里云 VPC 网络。

## 操作步骤 {#section_kjo_zp5_aq4 .section}

1.  **购买实例**

    1.  登录[消息队列 for Apache Kafka 控制台](http://kafka.console.aliyun.com/)。
    2.  在控制台的顶部导航栏，选择您将要购买和部署实例的地域（下图举例为**华东1（杭州）**，您可以根据需求进行切换）。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353137_zh-CN.png)

    3.  在消息队列 for Apache Kafka 控制台，单击左侧导航栏中的**概览**。

    4.  在实例列表页，单击**购买新实例**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353138_zh-CN.png)

    5.  在实例购买页，根据自身业务需求选择相应的配置。注意，在**实例类型**一栏，选择**公网/VPC实例**类型。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353139_zh-CN.png)

    6.  单击页面右侧的**立即购买**，按照提示完成购买流程。
2.  **获取 VPC 信息**

    1.  登录 [VPC 管理控制台](https://vpcnext.console.aliyun.com/)。在左侧导航栏，单击**交换机**。

    2.  在交换机页面，查看以下信息：

        -   VSwitch ID

        -   VPC ID

        -   可用区

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353140_zh-CN.png)

        **说明：** 请根据该页面的可用区（A～G）在消息队列 for Apache Kafka 控制台中选择对应的可用区（A～G）。例如，某 VPC 交换机（VSwitch）显示在**可用区E**，那么在消息队列 for Apache Kafka 控制台中就相应地选择**可用区E**。

3.  **部署实例**

    1.  在消息队列 for Apache Kafka 控制台，单击左侧导航栏的**概览**，查看已购买的实例。

    2.  选择处于**待部署**状态的实例，单击右侧的**部署**。

    3.  在部署对话框中：

        1.  在 **VPC ID**一栏，选择 VPC 的 ID。

        2.  在 **VSwitch ID** 一栏，填写在步骤二中获取的 VPC 的交换机 ID。

        3.  在**重新设置用户名密码**一栏：

            -   **否**：若无多实例共享用户名和密码的需求，选择该项，即使用系统为每个实例分配的默认用户名和密码。

            -   **是**：若您需要多实例共享同一用户名和密码，选择该项，然后自定义用户名和密码。

                ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353141_zh-CN.png)

                完成后，实例会进入**部署中**状态。实例部署预计需要 10~30 分钟。

    4.  刷新控制台页面。实例状态显示**服务中**，即表示已部署成功。
    u

4.  **查看实例详情**

    1.  实例部署成功后，实例类型显示为**公网/VPC实例**。单击该实例右侧的**详情**，查看实例详情。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353142_zh-CN.png)

        **说明：** 您也可在左侧导航栏单击**实例详情**，然后直接在页面上方选择要查看的实例，单击查看其详情。

    2.  在实例详情页：

        1.  查看该实例的**默认接入点**和**SSL接入点** 。

            -   从 **VPC** 内接入消息队列 for Apache Kafka 服务，需使用**默认接入点**。

            -   从**公网**接入消息队列 for Apache Kafka 服务，需使用 **SSL接入点**。

        2.  若之前部署实例时重新设置了**用户名**和**密码**，查看重设的用户名和密码。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998823/156861530353143_zh-CN.png)


