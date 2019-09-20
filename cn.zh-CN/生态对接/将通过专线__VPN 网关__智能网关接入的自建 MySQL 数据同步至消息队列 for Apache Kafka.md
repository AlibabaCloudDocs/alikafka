# 将通过专线/VPN 网关/智能网关接入的自建 MySQL 数据同步至消息队列 for Apache Kafka {#concept_120119_zh .concept}

使用数据传输服务 DTS（Data Transmission Service）的数据同步功能，您可以将通过专线/VPN 网关/智能网关接入的数据库的数据同步至消息队列 for Apache Kafka 集群，扩展消息处理能力。

## 前提条件 {#section_25x_v7a_m8j .section}

-   自建 MySQL 数据库版本为 5.1、5.5、5.6、5.7 版本。
-   已经将自建 MySQL 数据库通过专线/VPN网关/智能网关接入至阿里云专有网络，详情请参见[高速通道](https://help.aliyun.com/document_detail/44848.html)、[VPN网关](https://help.aliyun.com/document_detail/64960.html)、[智能接入网关](https://help.aliyun.com/document_detail/69227.html)。
-   已为[为自建MySQL创建账号并设置binlog](../../../../../cn.zh-CN/用户指南/准备工作（自建库）/为自建MySQL创建账号并设置binlog.md#) 。

## 注意事项 {#section_y6i_395_rhm .section}

-   如果同步对象为单个或多个表（非整库），那么在数据同步时，请勿对源库的同步对象使用 gh-ost 或 pt-online-schema-change 等类似工具执行在线 DDL 变更，否则会导致同步失败。
-   如果源数据库没有主键或唯一约束，且所有字段没有唯一性，可能会导致目标数据库中出现重复数据。

## 数据同步功能限制 {#section_mxf_r32_j59 .section}

-   同步对象仅支持数据表，不支持非数据表的对象。
-   不支持 DDL 操作的数据同步。
-   不支持自动调整同步对象。

    **说明：** 如果对同步对象中的数据表进行重命名操作，且重命名后的名称不在同步对象中，那么这部分数据将不再同步到到目标 Kafka 集群中。如需将修改后的数据表继续数据同步至目标 Kafka 集群中，您需要进行**修改同步对象**操作，详情请参考[修改同步对象](https://help.aliyun.com/document_detail/26634.html)。


## 支持同步的SQL操作 {#section_vdt_db0_b9r .section}

DML 操作：Insert、Update、Delete、Replace。

## 支持的同步架构 {#section_lh8_ate_bbq .section}

-   1 对 1 单向同步。
-   1 对多单向同步。
-   多对 1 单向同步。
-   级联同步。

## 消息格式 {#section_6oz_83a_ado .section}

同步到 Kafka 集群中的数据以 avro 格式存储，schema 定义详情请参考[DTS avro schema定义](https://github.com/LioRoger/subscribe_example/tree/master/avro)。

在数据同步到 Kafka 集群后，您需要根据 avro schema 定义进行数据解析。

## 操作步骤 {#section_ybn_t4q_g0e .section}

1.  [购买数据同步作业](../../../../../cn.zh-CN/快速入门/购买流程.md#)。

    **说明：** 购买时，选择源实例为 **MySQL**，目标实例为 **Kafka**，并选择同步拓扑为**单向同步**。

2.  登录[数据传输控制台](https://dts.console.aliyun.com/)。
3.  在左侧导航栏，单击**数据同步**。
4.  定位至已购买的数据同步实例，单击**配置同步链路**。
5.  配置同步通道的源实例及目标实例信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998865/156896514161667_zh-CN.png)

    |配置项目|配置选项|配置说明|
    |:---|:---|:---|
    |任务名称|-|     -   DTS 为每个任务自动生成一个任务名称，任务名称没有唯一性要求。
    -   您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。
 |
    |源实例信息|实例类型|选择**ECS 上的自建数据库**。|
    |实例地区|购买数据同步实例时选择的源实例地域信息，不可变更。|
    |ECS 实例 ID|选择作为同步数据源的 ECS 实例 ID。|
    |数据库类型|购买数据同步实例时选择的数据库类型，不可变更。|
    |端口|填入自建数据库服务端口，默认为 3306。|
    |数据库账号|填入连接 ECS 上自建 MySQL 数据库的账号，需要具备 Replication slave、Replication client 及所有同步对象的 Select 权限。|
    |数据库密码|填入连接 ECS 上自建 MySQL 数据库账号对应的密码。|
    |目标实例信息|实例类型|     -   Kafka 集群部署在 ECS 上时，选择**ECS 上的自建数据库**
    -   Kafka集群部署在本地服务器时，选择**通过专线/VPN 网关/智能网关接入的自建数据库**。

**说明：** 选择**通过专线/VPN 网关/智能网关接入的自建数据库**时，您需要配置**VPC ID**、**IP 地址**和**端口**。

 |
    |实例地区|购买数据同步实例时选择的目标实例地域信息，不可变更。|
    |ECS 实例 ID|选择部署了 Kafka 集群的 ECS 实例 ID。|
    |数据库类型|选择为**Kafka**。|
    |端口|Kafka 集群对外提供服务的端口，默认为 9092。|
    |数据库账号|填入 Kafka 集群的用户名，如 Kafka 集群未开启验证可不填写。|
    |数据库密码|填入 Kafka 集群用户名对应的密码，如 Kafka 集群未开启验证可不填写。|
    |Topic|     1.  单击击右侧的**获取 Topic 列表**。
    2.  下拉选择具体的 Topic 名称。
 |
    |Kafka 版本|根据目标 Kafka 集群版本，选择对应的版本信息。|

6.  单击页面右下角的**授权白名单并进入下一步**。
7.  配置同步对象信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998865/156896514161668_zh-CN.png)

    **说明：** 

    -   同步对象的粒度为表级别。
    -   在源库对象区域框中，选择需要同步的数据表，单击[![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/75938/154823638433712_zh-CN.png)](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/75938/154823638433712_zh-CN.png)移动到已选对象区域框中。
    -   默认情况下，对象迁移到Kafka集群后，对象名与 RDS 数据表一致。如果您迁移的对象在源数据库跟目标实例上名称不同，那么需要使用 DTS 提供的对象名映射功能，使用方法请参考[库表列映射](https://help.aliyun.com/document_detail/26628.html)。
8.  上述配置完成后单击页面右下角的**下一步**。
9.  配置同步初始化的高级配置信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998865/156896514161669_zh-CN.png)

    **说明：** 

    -   此步骤会将源实例中已经存在同步对象的结构及数据在目标实例中初始化，作为后续增量同步数据的基线数据。
    -   同步初始化类型细分为：结构初始化，全量数据初始化。默认情况下，需要选择**结构初始化**和**全量数据初始化**。
10. 上述配置完成后，单击页面右下角的**预检查并启动**。

    **说明：** 

    -   在数据同步任务正式启动之前，会先进行预检查。只有预检查通过后，才能成功启动数据同步任务。
    -   如果预检查失败，单击具体检查项后的![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134337/156896514139870_zh-CN.png)，查看具体的失败详情。根据失败原因修复后，重新进行预检查。
11. 在预检查对话框中显示**预检查通过**后，关闭预检查对话框，数据同步任务正式开始。

    您可以在数据同步页面，查看数据同步状态。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998865/156896514161671_zh-CN.png)


