---
keyword: [topic, metadata, 迁移, kafka]
---

# 迁移Topic上云

本教程介绍如何使用消息队列Kafka版提供的迁移工具将自建Kafka集群的Topic迁移到消息队列Kafka版实例。

您已完成以下操作：

-   [下载JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [下载迁移工具JAR文件](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**说明：**

-   迁移不会删除自建的源Kafka集群的Topic，只是在目标消息队列Kafka版实例创建相同配置的Topic。
-   迁移内容仅为Topic配置，不包含Topic中存储的消息。

1.  打开命令行工具。

2.  使用cd将路径切换到迁移工具所在目录。

3.  执行以下命令确认要迁移的Topic。

    `java -jar kafka-migration.jar TopicMigrationFromZk --sourceZkConnect 192.168.XX.XX --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId>`

    |参数|描述|
    |--|--|
    |sourceZkConnect|自建的源ZooKeeper集群的IP地址|
    |destAk|目标消息队列Kafka版实例所属阿里云账号的AccessKey ID|
    |destSk|目标消息队列Kafka版实例所属阿里云账号的AccessKey Secret|
    |destRegionId|目标消息队列Kafka版实例的地域ID|
    |destInstanceId|目标消息队列Kafka版实例的ID|

    待确认的返回结果示例如下：

    ```
    13:40:08 INFO - Begin to migrate topics:[test]
    13:40:08 INFO - Total topic number:1
    13:40:08 INFO - Will create topic:test, isCompactTopic:false, partition number:1
    ```

4.  执行以下命令提交要迁移的Topic。

    `java -jar kafka-migration.jar TopicMigrationFromZk --sourceZkConnect 192.168.XX.XX --destAk <yourAccessKeyId> --destSk <yourAccessKeySecret> --destRegionId <yourRegionID> --destInstanceId <yourInstanceId> --commit`

    |参数|描述|
    |--|--|
    |commit|提交迁移|

    提交迁移的返回结果示例如下：

    ```
    13:51:12 INFO - Begin to migrate topics:[test]
    13:51:12 INFO - Total topic number:1
    13:51:13 INFO - cmd=TopicMigrationFromZk, request=null, response={"code":200,"requestId":"7F76C7D7-AAB5-4E29-B49B-CD6F1E0F508B","success":true,"message":"operation success"}
    13:51:13 INFO - TopicCreate success, topic=test, partition number=1, isCompactTopic=false
    ```

5.  确认Topic迁移是否成功。

    1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

    2.  在顶部菜单栏，选择目标实例所在地域。

    3.  在左侧导航栏，单击**Topic管理**。

    4.  在**Topic管理**页面，选择目标实例。

        **Topic**列表显示成功迁移的Topic。


