# 创建MySQL Source Connector

本文介绍如何创建MySQL Source Connector，通过DataWorks将数据从阿里云数据库RDS MySQL版导出至消息队列Kafka版实例的Topic。

在导出数据前，请确保您已完成以下操作：

-   为消息队列Kafka版实例开启Connector。更多信息，请参见[开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)。

    **说明：** 请确保您的消息队列Kafka版实例部署在华南1（深圳）、西南1（成都）、华北2（北京）、华北3（张家口）、华东1（杭州）或华东2（上海）地域。

-   [创建RDS MySQL实例](/cn.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)。
-   [创建数据库和账号](/cn.zh-CN/RDS MySQL 数据库/快速入门/创建数据库和账号.md)。
-   创建数据库表。常见的SQL语句，请参见[常用语句](/cn.zh-CN/RDS MySQL 数据库/附录/常用 SQL 命令（MySQL）.md)。
-   阿里云账号和RAM用户均须授予DataWorks访问您弹性网卡ENI资源的权限。授予权限，请访问[云资源访问授权](https://ram.console.aliyun.com/role/authorization?request=%7B%22Services%22%3A%5B%7B%22Service%22%3A%22DataWorks%22%2C%22Roles%22%3A%5B%7B%22RoleName%22%3A%22AliyunDataWorksAccessingENIRole%22%2C%22TemplateId%22%3A%22ENIRole%22%7D%5D%7D%5D%2C%22ReturnUrl%22%3A%22https%253A%252F%252Fkafka.console.aliyun.com%22%7D)。

    **说明：** 如果您使用的是RAM用户，请确保您的账号有以下权限：

    -   AliyunDataWorksFullAccess：DataWorks所有资源的管理权限。
    -   AliyunBSSOrderAccess：购买阿里云产品的权限。
    如何为RAM用户添加权限策略，请参见[步骤二：为RAM用户添加权限](/cn.zh-CN/权限控制/RAM主子账号授权.md)。

-   请确保您是阿里云数据库RDS MySQL版实例（数据源）和消息队列Kafka版实例（数据目标）的所有者，即创建者。
-   请确保阿里云数据库RDS MySQL版实例（数据源）和消息队列Kafka版实例（数据目标）所在的VPC网段没有重合，否则无法成功创建同步任务。

您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)创建数据同步任务，将您在阿里云数据库RDS MySQL版数据库表中的数据同步至消息队列Kafka版的Topic。该同步任务将依赖阿里云DataWorks产品实现，流程图如下所示。

![mysql_connector](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8723574161/p245245.png)

如果您在消息队列Kafka版控制台成功创建了数据同步任务，那么阿里云DataWorks会自动为您开通DataWorks产品基础版服务（免费）、新建DataWorks项目（免费）、并新建数据集成独享资源组（**需付费**），资源组规格为4c8g，购买模式为包年包月，时长为1个月并自动续费。阿里云DataWorks的计费详情，请参见[DataWorks计费概述]()。

此外，DataWorks会根据您数据同步任务的配置，自动为您生成消息队列Kafka版的目标Topic。数据库表和Topic是一对一的关系，对于有主键的表，默认6分区；无主键的表，默认1分区。请确保实例剩余Topic数和分区数充足，不然任务会因为创建Topic失败而导致异常。

Topic的命名格式为`<配置的前缀>_<数据库表名>`，下划线（\_）为系统自动添加的字符。详情如下图所示。

![table_topic_match](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8723574161/p245247.png)

例如，您将前缀配置为mysql，需同步的数据库表名为table\_1，那么DataWorks会自动为您生成专用Topic，用来接收table\_1同步过来的数据，该Topic的名称为mysql\_table\_1；table\_2的专用Topic名称为mysql\_table\_2，以此类推。

## 注意事项

-   地域说明
    -   如果数据源和目标实例位于不同地域，请确保您使用的账号拥有云企业网实例，且云企业网实例已挂载数据源和目标实例所在的VPC，并配置好流量带宽完成网络打通。

        否则，可能会新建云企业网实例，并将目标实例和独享资源组ECS全部挂载到云企业网实例来打通网络。这样的云企业网实例没有配置带宽，所以带宽流量很小，可能导致创建同步任务过程中的网络访问出错，或者同步任务创建成功后，在运行过程中出错。

    -   如果数据源和目标实例位于同一地域，创建数据同步任务会自动在其中一个实例所在VPC创建ENI，并绑定到独享资源组ECS上，以打通网络。
-   DataWorks独享资源组说明
    -   DataWorks的每个独享资源组可以运行最多3个同步任务。创建数据同步任务时，如果DataWorks发现您的账号名下有资源组的历史购买记录，并且运行的同步任务少于3个，将使用已有资源组运行新建的同步任务。
    -   DataWorks的每个独享资源组最多绑定两个VPC的ENI。如果DataWorks发现已购买的资源组绑定的ENI与需要新绑定的ENI有网段冲突，或者其他技术限制，导致使用已购买的资源组无法创建出同步任务，此时，即使已有的资源组运行的同步任务少于3个，也将新建资源组确保同步任务能够顺利创建。

## 创建并部署MySQL Source Connector

1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)。

2.  在顶部菜单栏，选择地域。

3.  在左侧导航栏，单击**实例列表**。

4.  在**实例列表**页面，单击目标实例名称。

5.  在左侧导航栏，单击**Connector（公测组件）**。

6.  在**Connector（公测组件）**页面，单击**创建Connector**。

7.  在**创建Connector**配置向导中，完成以下操作。

    1.  在**基础信息**页签的**Connector名称**文本框，输入Connector名称，然后将**转储路径**配置为从**云数据库RDS MySQL版**转储到**消息队列Kafka版**，单击**下一步**。

        **说明：** 消息队列Kafka版会为您自动选中**授权创建服务关联角色**。

        -   如果未创建服务关联角色，消息队列Kafka版会为您自动创建一个服务关联角色，以便您将云数据库RDS MySQL版的数据导出至消息队列Kafka版。
        -   如果已创建服务关联角色，消息队列Kafka版不会重复创建。
        关于该服务关联角色的更多信息，请参见[服务关联角色](/cn.zh-CN/权限控制/服务关联角色.md)。

        |参数|描述|示例值|
        |--|--|---|
        |**Connector名称**|Connector的名称。命名规则：        -   可以包含数字、小写英文字母和短划线（-），但不能以短划线（-）开头，长度限制为48个字符。
        -   同一个消息队列Kafka版实例内保持唯一。
Connector的数据同步任务必须使用名称为connect-任务名称的Consumer Group。如果您未手动创建该Consumer Group，系统将为您自动创建。

|kafka-source-mysql|
        |**转储路径**|配置数据转储的源和目标。第一个下拉列表中选择数据源，第二个下拉列表中选择目标。|从**云数据库RDS MySQL版**转储到**消息队列Kafka版**|

    2.  在**源实例配置**页签，配置以下参数，然后单击**下一步**。

        |参数|描述|示例值|
        |--|--|---|
        |**RDS实例ID**|需要同步数据的阿里云数据库RDS MySQL版的实例ID。|rm-wz91w3vk6owmz\*\*\*\*|
        |**RDS实例所在地域**|从下拉列表中，选择阿里云数据库RDS MySQL版实例所在的地域的ID。|cn-shenzhen|
        |**RDS实例数据库**|需要同步的阿里云数据库RDS MySQL版实例数据库的名称。|mysql-to-kafka|
        |**RDS实例数据库账号**|需要同步的阿里云数据库RDS MySQL版实例数据库账号。|mysql\_to\_kafka|
        |**RDS实例账号密码**|需要同步的阿里云数据库RDS MySQL版实例数据库账号的密码。|无|
        |**RDS数据库表**|需要同步的阿里云数据库RDS MySQL版实例数据库表的名称，多个表名以英文逗号（,）分隔。数据库表和目标Topic是一对一的关系。

|mysql\_tbl|
        |**命名前缀**|阿里云数据库RDS MySQL版数据库表同步到消息队列Kafka版的Topic的命名前缀，请确保前缀全局唯一。|mysql|

    3.  在**目标实例配置**页签，会显示数据将同步到的目标消息队列Kafka版实例，确认信息无误后，单击**下一步**。

    4.  在**预览/提交**页签，确认Connector的配置，然后单击**提交**。

8.  在**创建Connector**面板，单击**部署**。

    在**Connector（公测组件）**页面，您可以看到创建的任务状态为**运行中**，则说明任务创建成功。

    **说明：** 如果创建失败，请再次检查本文前提条件中的操作是否已全部完成。


## 验证结果

1.  向阿里云数据库RDS MySQL版数据库表插入数据。

    示例如下。

    ```
    INSERT INTO mysql_tbl
        (mysql_title, mysql_author, submission_date)
        VALUES
        ("mysql2kafka", "tester", NOW())
    ```

    更多SQL语句，请参见[常用语句](/cn.zh-CN/RDS MySQL 数据库/附录/常用 SQL 命令（MySQL）.md)。

2.  使用消息队列Kafka版提供的消息查询功能，验证数据能否被导出至消息队列Kafka版目标Topic。

    查询的具体步骤，请参见[查询消息](/cn.zh-CN/用户指南/查询消息.md)。

    云数据库RDS MySQL版数据库表导出至消息队列Kafka版Topic的数据示例如下。

    ```
    {
        "schema":{
            "dataColumn":[
                {
                    "name":"mysql_id",
                    "type":"LONG"
                },
                {
                    "name":"mysql_title",
                    "type":"STRING"
                },
                {
                    "name":"mysql_author",
                    "type":"STRING"
                },
                {
                    "name":"submission_date",
                    "type":"DATE"
                }
            ],
            "primaryKey":[
                "mysql_id"
            ],
            "source":{
                "dbType":"MySQL",
                "dbName":"mysql_to_kafka",
                "tableName":"mysql_tbl"
            }
        },
        "payload":{
            "before":null,
            "after":{
                "dataColumn":{
                    "mysql_title":"mysql2kafka",
                    "mysql_author":"tester",
                    "submission_date":1614700800000
                }
            },
            "sequenceId":"1614748790461000000",
            "timestamp":{
                "eventTime":1614748870000,
                "systemTime":1614748870925,
                "checkpointTime":1614748870000
            },
            "op":"INSERT",
            "ddl":null
        },
        "version":"0.0.1"
    }
    ```


