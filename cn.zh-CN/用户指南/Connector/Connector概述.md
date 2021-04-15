---
keyword: [kafka, connector]
---

# Connector概述

消息队列Kafka版提供全托管、免运维的Connector，用于消息队列Kafka版和其他阿里云服务之间的数据同步。本文介绍Connector支持的数据同步任务的类型、使用流程、使用限制以及跨地域数据同步。

**说明：** 消息队列Kafka版的Connector组件处于公测阶段，且独立于消息队列Kafka版实例，因此不会在消息队列Kafka版侧产生费用。同时，阿里云不承诺Connector的SLA，使用Connector所依赖的其他产品的SLA和费用说明请以对应产品为准。

## Connector类型

消息队列Kafka版支持以下两大类的Connector：

-   Sink Connector：Sink代表数据向外流转，即消息队列Kafka版为数据源，其他产品为数据目标。

    |Connector|描述|文档|
    |---------|--|--|
    |FC Sink Connector|将数据从消息队列Kafka版导出至函数计算。|[创建FC Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)|
    |MaxCompute Sink Connector|将数据从消息队列Kafka版导出至大数据计算服务MaxCompute。|[创建MaxCompute Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)|
    |OSS Sink Connector|将数据从消息队列Kafka版导出至对象存储OSS。|[创建OSS Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建OSS Sink Connector.md)|
    |Elasticsearch Sink Connector|将数据从消息队列Kafka版导出至阿里云Elasticsearch。|[创建Elasticsearch Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建Elasticsearch Sink Connector.md)|

-   Source Connector：Source代表数据向内流转，即消息队列Kafka版为数据目标，其他产品为数据源。

    |Connector|描述|文档|
    |---------|--|--|
    |MySQL Source Connector|将数据从阿里云数据库RDS MySQL版导出至消息队列Kafka版。|[创建MySQL Source Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MySQL Source Connector.md)|


## 使用流程

Connector的使用流程如下：

1.  [开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)
2.  创建Connector
    -   [创建FC Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)
    -   [创建MaxCompute Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)
    -   [创建OSS Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建OSS Sink Connector.md)
    -   [创建Elasticsearch Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建Elasticsearch Sink Connector.md)
    -   [创建MySQL Source Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MySQL Source Connector.md)
3.  更多Connector的相关操作
    -   [查看Connector任务配置](/cn.zh-CN/用户指南/Connector/查看Connector任务配置.md)
    -   [查看Connector日志](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)
    -   [暂停Connector](/cn.zh-CN/用户指南/Connector/暂停Connector.md)
    -   [恢复Connnector](/cn.zh-CN/用户指南/Connector/恢复Connnector.md)
    -   [删除Connector](/cn.zh-CN/用户指南/Connector/删除Connector.md)
    -   [修改Connector配置](/cn.zh-CN/用户指南/Connector/修改Connector配置.md)
    -   [测试Connector](/cn.zh-CN/用户指南/Connector/测试Connector.md)
    -   [为Connector开启公网访问](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)

## 使用限制

消息队列Kafka版对Connector的限制如下：

|项目|限制值|
|--|---|
|数量|单实例最多创建3个|
|地域|-   华东1（杭州）
-   华东2（上海）
-   华北2（北京）
-   华北3（张家口）
-   华北5（呼和浩特）
-   华南1（深圳）
-   西南1（成都）
-   中国香港
-   新加坡（新加坡）
-   日本（东京） |

**说明：** 如果您需要提升您的实例的Connector的数量限制或者需要更多的地域，请提交工单[工单](https://selfservice.console.aliyun.com/ticket/category/alikafka)联系消息队列Kafka版技术人员。

## 跨地域数据同步

如果您需要将某个地域的数据，通过Connector同步到另一个地域的阿里云服务，您需要为该Connector开启公网访问，然后在公网进行数据同步。具体操作步骤，请参见[为Connector开启公网访问](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。

**说明：** MySQL Source Connector的跨地域数据同步比较特殊，需要您自行开通企业网。更多信息，请参见[创建MySQL Source Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MySQL Source Connector.md)。

