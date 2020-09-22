---
keyword: [topic, metadata, 迁移, kafka]
---

# 云上迁移Topic

本教程介绍如何使用消息队列Kafka版提供的迁移工具将某个消息队列Kafka版实例的Topic迁移到另一个消息队列Kafka版实例。

您已完成以下操作：

-   [下载JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [下载迁移工具JAR文件](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**说明：**

-   迁移不会删除源消息队列Kafka版实例的Topic，只是在目标消息队列Kafka版实例创建相同配置的Topic。
-   迁移内容仅为Topic配置，不包含Topic中存储的数据。

1.  打开命令行工具。

2.  使用cd将路径切换到迁移工具所在目录。

3.  执行以下命令确认要迁移的Topic。

    `java -jar kafka-migration.jar TopicMigrationFromAliyun --sourceAk <yoursourceAccessKeyId> --sourceSk <yoursourceAccessKeySecret> --sourceRegionId <yoursourceRegionId> --sourceInstanceId <yoursourceInstanceId> --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId>`

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
    15:13:12 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"total":4,"code":200,"requestId":"1CBAB340-2146-43A3-8470-84D77DB8B43E","success":true,"pageSize":10000,"currentPage":1,"message":"operation success.","topicList":[{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558314000,"regionId":"cn-hangzhou","statusName":"服务中","topic":"agdagasdg","remark":"agdadgdasg","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558294000,"regionId":"cn-hangzhou","statusName":"服务中","topic":"135215","remark":"1315215","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558214000,"regionId":"cn-hangzhou","statusName":"服务中","topic":"1332","remark":"13414","partitionNum":12,"compactTopic":false,"status":0,"tags":[]},{"instanceId":"alikafka_pre-cn-0pp1cng20***","localTopic":false,"createTime":1578558141000,"regionId":"cn-hangzhou","statusName":"服务中","topic":"aete","remark":"est","partitionNum":12,"compactTopic":false,"status":0,"tags":[]}]}
    15:13:12 INFO - Will create topic:agdagasdg, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:135215, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:1332, isCompactTopic:false, partition number:12
    15:13:12 INFO - Will create topic:aete, isCompactTopic:false, partition number:12
    ```

4.  执行以下命令提交要迁移的Topic。

    `java -jar kafka-migration.jar TopicMigrationFromAliyun --sourceAk <yoursourceAccessKeyId> --sourceSk <yoursourceAccessKeySecret> --sourceRegionId <yoursourceRegionId> --sourceInstanceId <yoursourceInstanceId> --destAk <yourdestAccessKeyId> --destSk <yourdestAccessKeySecret> --destRegionId <yourdestRegionId> --destInstanceId <yourdestInstanceId> --commit`

    |参数|描述|
    |--|--|
    |commit|提交迁移|

    提交迁移的返回结果示例如下：

    ```
    16:38:30 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"A0CA4D70-46D4-45CF-B9E0-B117610A26DB","success":true,"message":"operation success"}
    16:38:30 INFO - TopicCreate success, topic=agdagasdg, partition number=12, isCompactTopic=false
    16:38:36 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"05E88C75-64B6-4C87-B962-A63D906FD993","success":true,"message":"operation success"}
    16:38:36 INFO - TopicCreate success, topic=135215, partition number=12, isCompactTopic=false
    16:38:42 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"9D54F6DB-6FA0-4F6D-B19A-09109F70BDDA","success":true,"message":"operation success"}
    16:38:42 INFO - TopicCreate success, topic=1332, partition number=12, isCompactTopic=false
    16:38:49 INFO - cmd=TopicMigrationFromAliyun, request=null, response={"code":200,"requestId":"6C265013-D15E-49AE-BE55-BF7657ADA1B7","success":true,"message":"operation success"}
    16:38:49 INFO - TopicCreate success, topic=aete, partition number=12, isCompactTopic=false
    ```

5.  确认Topic迁移是否成功。

    1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

    2.  在顶部菜单栏，选择目标实例所在地域。

    3.  在左侧导航栏，单击**Topic管理**。

    4.  在**Topic管理**页面，选择目标实例。

        **Topic**列表显示成功迁移的Topic。


