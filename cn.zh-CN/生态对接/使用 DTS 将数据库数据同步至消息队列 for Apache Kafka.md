# 使用 DTS 将数据库数据同步至消息队列 for Apache Kafka {#concept_120119_zh .concept}

使用数据传输服务 DTS（Data Transmission Service）的数据同步功能，您可以将通过专线/VPN 网关/智能网关接入的数据库的数据同步至消息队列 for Apache Kafka 集群，扩展消息处理能力。

[立即购买消息队列 for Apache Kafka\>\>](https://common-buy.aliyun.com/?spm=5176.kafka.Index.1.793625e8nUylkF&commodityCode=alikafka_pre&regionId=cn-hangzhou#/buy)

具体的前提条件、注意事项、操作步骤等信息，请参见[从通过专线/VPN网关/智能网关接入的自建MySQL同步至自建Kafka集群](../../../../cn.zh-CN/用户指南/实时同步/MySQL实时同步至其他数据库/从通过专线__VPN网关__智能网关接入的自建MySQL同步至自建Kafka集群.md#)。

当您操作到[操作步骤](../../../../cn.zh-CN/用户指南/实时同步/MySQL实时同步至其他数据库/从通过专线__VPN网关__智能网关接入的自建MySQL同步至自建Kafka集群.md#section_v5h_m5c_zgb)时，请注意以下事项：

|字段|说明|
|--|--|
|实例类型|选择**通过专线/VPN网关/智能网关接入的自建数据库**。|
|对端专有网络|选择消息队列 for Apache Kafka 实例的 VPC。|
|IP 地址|选择消息队列 for Apache Kafka 实例接入点的任意一个 IP 地址即可，目前仅仅支持填写单个 IP 地址。|
|端口|选择消息队列 for Apache Kafka 实例接入点对应 IP 的对应端口。|
|数据库账号|非必填项。|
|数据库密码|非必填项。|
|Topic|获取 Topic 列表后选择对应 Topic，Topic 建议创建单个分区，以便保证全局顺序。|
|Kafka版本|选择消息队列 for Apache Kafka 实例对应的开源版本，目前主要是 0.10 版本。|

**说明：** 当前 DTS 仅支持通过默认接入点导入数据到消息队列 for Apache Kafka，假设消息队列 for Apache Kafka 实例的默认接入点为 “172.16.X.X1:9092,172.16.X.X2:9092,172.16.X.X3:9092”，选择第一个 IP 地址和端口填写即可，也即上表中的 **IP地址**填写 “172.16.X.X1”，**端口**填写 “9092”。

配置结果如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998866/156837245253223_zh-CN.png)

