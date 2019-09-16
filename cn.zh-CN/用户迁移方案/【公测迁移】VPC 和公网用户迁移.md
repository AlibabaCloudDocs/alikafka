# 【公测迁移】VPC 和公网用户迁移 {#concept_89773_zh .concept}

为了保证服务质量和用户体验，消息队列 for Apache Kafka 商业化后将提供用户专有的商业化实例，不再提供公测版的共享实例服务。

**说明：** 

公测期间所有实例均为共享类型，供所有用户使用；公测结束后（预计在 2019 年 4 月上旬结束公测），您需购买您的实例，可以是 VPC 或公网类型。购买后，您将不再与其他用户共享实例。

本文以 VPC 网络类型的实例为例。公网接入的更多信息请参见[公网 + VPC 接入](../cn.zh-CN/快速入门/步骤二：购买和部署实例/公网 + VPC 接入.md#)。

-   若您已经是 VPC 的用户，请按照以下步骤将现有业务迁移到消息队列 for Apache Kafka 商用实例上。

-   若您目前是经典网络的用户，请参见[【公测迁移】经典网络用户迁移](cn.zh-CN/用户迁移方案/【公测迁移】经典网络用户迁移.md#)迁移业务。


## 步骤一：购买消息队列 for Apache Kafka 实例 {#section_pbi_os1_u5w .section}

前往[产品购买页](https://common-buy.aliyun.com/?commodityCode=alikafka_pre#/buy)，请根据业务所在地域购买相应的实例。

## 步骤二：部署消息队列 for Apache Kafka 实例 {#section_4nw_uka_2bw .section}

1.  登录[消息队列 for Apache Kafka 控制台](http://kafka.console.aliyun.com/)，在概览页查看已购买的实例。

2.  选择处于**未部署**状态的实例，单击**部署**按钮，然后根据页面提示填写 VPC 信息。

    完成后，实例会进入**部署中**状态。实例部署预计需要 10 分钟 ~ 30 分钟。

    **说明：** 获取需要填写的 VPC 信息的步骤请参见 [VPC 接入](../cn.zh-CN/快速入门/步骤二：购买和部署实例/VPC 接入.md#)。

3.  刷新控制台页面。若实例状态显示**服务中**，即表示已部署成功。


## 步骤三：创建 Topic {#section_sbf_wmj_klo .section}

消息主题（Topic）是消息队列 for Apache Kafka 里对消息的一级归类，比如可以创建“Topic\_Trade”这一主题用来识别交易类消息。 使用消息队列 for Apache Kafka 的第一步就是先为您的应用创建 Topic。

请按照以下步骤创建 Topic：

1.  在消息队列 for Apache Kafka 控制台的左侧导航栏中，单击**Topic管理** 。

2.  在 Topic管理页面的上方选择相应的地域，例如**华北2（上海）**，然后单击**创建Topic**按钮。

3.  在创建Topic 页面，输入 Topic 名称和备注并选择已部署的新实例，然后单击**确认**。


完成后，您创建的 Topic 将出现在Topic 管理 页面的列表中。

**说明：** Topic 不能跨地域使用，需要在应用程序所在的地域（即所部署的 ECS 的所在地域）进行创建。比如 Topic 创建在**华北2（上海）**这个地域，那么消息生产端和消费端也必须运行在**华北2（上海）**的 ECS 上。地域的详细介绍请参见[地域和可用区](../../../../../cn.zh-CN/通用参考/地域和可用区.md#)。

## 步骤四：创建 Consumer Group {#section_nti_shy_3go .section}

Consumer Group 是一类 Consumer 的标识，这类 Consumer 通常接收并消费同一类消息，且消费逻辑一致。Consumer Group 和 Topic 的关系是 N：N。 同一个 Consumer Group 可以订阅多个 Topic，同一个 Topic 也可以被多个 Consumer Group 订阅。

创建完 Topic 后，请按以下步骤创建 Consumer Group：

1.  在消息队列 for Apache Kafka 控制台的左侧导航栏中，单击**Consumer Group管理** 。

2.  在Consumer Group管理 页面的上方选择相应的地域，例如**华北2（上海）******，单击**创建Consumer Group** 按钮。

3.  在创建Consumer Group 页面，填写 Consumer Group 的名称，单击**创建**。


完成后，您创建的 Consumer Group 将出现在Consumer Group 管理 页面的列表中。

## 步骤五：获取实例接入点 {#section_4xz_xl7_0ru .section}

实例的接入点是您在使用 SDK 接入消息队列 for Apache Kafka 时需要配置的一个配置项。如果您选择迁移到您新部署的实例，则需获取您新部署的实例的接入点。

同一个实例的接入点一致，因此可任意选择该实例的资源（Topic 或 Consumer Group）获取接入点。

请按以下步骤获取您新部署的实例的接入点：

1.  进入控制台的 Topic 管理或 Consumer Group管理页面。

2.  单击 Topic 或 Consumer Group **操作**列中的**获取接入点**。

3.  在接入点页面，单击**复制**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998815/156861524153116_zh-CN.png)


## 步骤六：修改代码配置，执行迁移 {#section_l7i_1sc_uwq .section}

消息队列 for Apache Kafka 提供以下两种迁移方案：

-   **方案一**：若您的业务允许丢弃少量未消费的消息数据，那么，您可以直接将生产者和消费者更新为您新部署实例的配置，即可完成迁移。

-   **方案二**：若您的业务对消息数据比较敏感，那么，您需要在保证消息消费完成的情况下，完成业务的平滑迁移。


方案二的平滑迁移步骤如下：

1.  将生产者更新为新部署实例的配置，则新消息写入新部署实例。

2.  确保消费者在共享实例上的既存消息已消费完成。

3.  将消费者更新为新部署实例的配置，开始消费新部署实例上的消息。


请按照以下示例代码修改配置文件`kafka.properties`，以更新至新部署的实例相关配置：

``` {#codeblock_13y_dlk_167}
### Java（Python、NodeJS 可同理参考)
1. bootstrap.server 修改：旧接入点(如 kafka-cn-internet.aliyun.com:8080) => 新部署实例的接入点(如 192.168.0.1:9092,192.168.0.2:9092)；
2. security.protocol 修改："SASL_SSL" => "PLAINTEXT"；


### Go
1. servers 修改：旧接入点(比如kafka-cn-internet.aliyun.com:8080) => 新集群的接入点(比如192.168.0.1:9092,192.168.0.2:9092)
2. clusterCfg.Net.SASL.Enable 修改：true => false
3. clusterCfg.Net.TLS.Enable 修改：true => false
			
```

## 步骤七：验证迁移是否成功 {#section_lqu_zhc_z0i .section}

如果生产者能成功发送且消费者能成功消费消息，则说明迁移成功。

**验证生产者是否能成功发送消息**

1.  在消息队列 for Apache Kafka 控制台左侧导航栏单击 **Topic管理**。
2.  在 Topic 的**操作**列单击**查看分区状态**。

若能看到**最近更新时间**有更新，则代表生产者已成功发送消息。

**验证消费者是否能成功消费消息**

1.  在消息队列 for Apache Kafka 控制台左侧导航栏单击 **Consumer Group管理**。
2.  在 Consumer Group 的**操作**列单击**查看消息堆积**。

若能看到**最近消费时间**有更新，则代表消费者已成功消费消息。

