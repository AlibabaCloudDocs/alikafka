---
keyword: [Consumer Group, metadata, 迁移, kafka]
---

# 云上迁移Consumer Group

本文介绍如何使用消息队列Kafka版提供的迁移工具将某个消息队列Kafka版实例的Consumer Group迁移到另一个消息队列Kafka版实例。

您已完成以下操作：

-   [下载JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [下载迁移工具JAR文件](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**说明：**

-   迁移不会删除源消息队列Kafka版实例的Consumer Group，只是在目标消息队列Kafka版实例创建相同配置的Consumer Group。
-   迁移内容仅为Consumer Group配置，不包含Consumer Group消费的Topic及位点信息。

1.  打开命令行工具。

2.  使用cd命令将路径切换到迁移工具所在目录。

3.  确认要迁移的Consumer Group。

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromAliyun --sourceAk <yoursourceAccessKeyId> --sourceSk <yoursourceAccessKeySecret> --sourceRegionId <yoursourceRegionId> --sourceInstanceId <yoursourceInstanceId> --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId>`

    |参数|描述|
    |--|--|
    |sourceAk|源消息队列Kafka版实例所属阿里云账号的AccessKey ID|
    |sourceSk|源消息队列Kafka版实例所属阿里云账号的AccessKey Secret|
    |sourceRegionId|源消息队列Kafka版实例的地域ID|
    |sourceInstanceId|源消息队列Kafka版实例的ID|
    |destAk|目标消息队列Kafka版实例所属阿里云账号的AccessKey ID|
    |destSk|目标消息队列Kafka版实例所属阿里云账号的AccessKey Secret|
    |destRegionId|目标消息队列Kafka版实例的地域ID|
    |destInstanceId|目标消息队列Kafka版实例的ID|

    待确认的返回结果示例如下：

    ```
    10:54:26 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"9793DADB-55A5-4D4E-9E9C-D4DA8B35370C","success":true,"consumerList":[{"instanceId":"alikafka_post-cn-0pp1h0uv6***","regionId":"cn-hangzhou","consumerId":"Demo","tags":[{"value":"","key":"migration"}]}],"message":"operation success."}
    10:54:26 INFO - Will create consumer groups:[Demo]
    ```

4.  执行以下命令提交要迁移的Consumer Group。

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromAliyun --sourceAk LTAI4FwQ5aK1mFYCspJ1h*** --sourceSk wvDxjjRQ1tHPiL0oj7Y2Z7WDNkS*** --sourceRegionId cn-hangzhou --sourceInstanceId alikafka_post-cn-0pp1h0uv6*** --destAk LTAI4FwQ5aK1mFYCspJ1h*** --destSk wvDxjjRQ1tHPiL0oj7Y2Z7*** --destRegionId cn-hangzhou --destInstanceId alikafka_pre-cn-v0h1cng00*** --commit`

    |参数|说明|
    |--|--|
    |commit|提交迁移|

    提交迁移的返回结果示例如下：

    ```
    10:54:40 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"49E53B79-3C2C-4BCF-8BC8-07B0BB14B52A","success":true,"consumerList":[{"instanceId":"alikafka_post-cn-0pp1h0uv6***","regionId":"cn-hangzhou","consumerId":"Demo","tags":[{"value":"","key":"migration"}]}],"message":"operation success."}
    10:54:41 INFO - cmd=ConsumerGroupMigrationFromAliyun, request=null, response={"code":200,"requestId":"5AEEFB13-2A6B-4265-97CB-902CFA483339","success":true,"message":"operation success"}
    10:54:41 INFO - ConsumerCreate success, consumer group=Demo
    ```

5.  确认Group迁移是否成功。

    1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

    2.  在**概览**页面的**资源分布**区域，选择地域。

    3.  在**实例列表**页面，单击目标实例名称。

    4.  在左侧导航栏，单击**Group 管理**。

    5.  在**Group 管理**页面的Group列表显示成功迁移的Group。


