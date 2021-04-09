# 如何快速测试消息队列Kafka版服务端是否正常？

在创建并部署消息队列Kafka版实例后，您可以使用消息队列Kafka版控制台直接发送消息，快速测试服务端是否正常。

您已创建并部署消息队列Kafka版实例，且实例处于**服务中**状态。

## 操作流程

快速测试消息队列Kafka版服务端的流程如下：

1.  [创建Topic](#section_jax_bs9_o5x)
2.  [发送消息](#section_ldk_ge6_y1v)
3.  [查看分区状态](#section_cyx_ddi_5vi)
4.  [按位点查询消息](#section_tar_w7j_afd)

您可以多次重复步骤2到步骤4，如果多次操作正常，则说明服务端正常。

**说明：** 如果服务端正常，但发送消息依然失败，建议您去调用方（例如，原生客户端、生态组件端等）排查问题。

## 创建Topic

创建用于接收消息的Topic。

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

2.  在左侧导航栏，单击**Topic管理**。

3.  在**Topic管理**页面顶部，选择目标实例，然后单击**创建Topic**。

    ![create topic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87665.png)

4.  在**创建Topic**对话框，设置Topic属性，然后单击**创建**。

    ![createtopic](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87632.png)

    字段说明如下：

    -   **Topic**：Topic名称，例如demo。
    -   **标签**：标签，例如demo。
    -   **实例**：实例ID，例如alikafka\_pre-cn-\*\*\*。
    -   **备注**：备注信息，例如demo。
    -   **分区数**：Topic的分区数量，例如12。

## 发送消息

往创建的Topic的指定分区发送消息。

1.  在**Topic管理**页面，找到创建的Topic，在其**操作**列，单击**发送消息**。

    ![sendmessage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87667.png)

2.  在**发送消息**对话框，设置分区和消息属性，然后单击**发送**。

    ![sendmessagesetting](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87671.png)

    字段说明如下：

    -   **分区**：分区ID，例如0。
    -   **Message Key**：消息键，例如demo。
    -   **Message Value**：消息值，例如 \{"key": "test"\}。

## 查看分区状态

往指定分区发送消息后，查看该分区的状态。

1.  在**Topic管理**页面，找到发送消息到的Topic，在其**操作**列，单击**分区状态**。

    ![sendmessage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87667.png)

2.  在**分区状态**对话框，单击**刷新**。

    ![update](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2606119951/p87686.png)


## 按位点查询消息

根据分区ID和位点查询消息。

1.  在左侧导航栏，单击**消息查询**。

2.  在**消息查询**页面，选择目标实例，单击**按位点查询**页签。

    1.  在**请输入Topic**文本框，输入Topic。

    2.  从**请选择分区**下拉列表，选择发送消息到的分区ID。

    3.  在**请输入位点**文本框，输入位点。

    4.  单击**搜索**。

    ![query message](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3606119951/p87737.png)


