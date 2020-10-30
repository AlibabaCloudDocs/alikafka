---
keyword: [kafka, connector]
---

# Connector概述

消息队列Kafka版提供全托管、免运维的Connector，用于同一地域内的消息队列Kafka版和其他阿里云服务之间的数据同步。Connector当前处于公测阶段。本文介绍Connector的类型、使用流程、使用限制以及跨地域数据同步。

## Connector类型

消息队列Kafka版目前仅提供Sink Connector，用于将数据从消息队列Kafka版导出至其他阿里云服务。消息队列Kafka版支持以下类型的Sink Connector：

|Sink Connector类型|任务类型|描述|文档|
|----------------|----|--|--|
|FC Sink Connector|KAFKA2FC|将数据从消息队列Kafka版导出至函数计算。|[创建FC Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)|
|MaxCompute Sink Connector|KAFKA2ODPS|将数据从消息队列Kafka版导出至大数据计算服务MaxCompute。|[创建MaxCompute Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)|

## 使用流程

Connector的使用流程如下：

1.  开启Connector

    [开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)

2.  创建Connector
    -   [创建MaxCompute Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建MaxCompute Sink Connector.md)
    -   [创建FC Sink Connector](/cn.zh-CN/用户指南/Connector/创建Connector/创建FC Sink Connector.md)
3.  管理Connector
    -   [查看Connector任务配置](/cn.zh-CN/用户指南/Connector/查看Connector任务配置.md)
    -   [查看Connector日志](/cn.zh-CN/用户指南/Connector/查看Connector日志.md)
    -   [暂停Connector](/cn.zh-CN/用户指南/Connector/暂停Connector.md)
    -   [恢复Connnector](/cn.zh-CN/用户指南/Connector/恢复Connnector.md)
    -   [删除Connector](/cn.zh-CN/用户指南/Connector/删除Connector.md)
    -   [修改Connector描述](/cn.zh-CN/用户指南/Connector/修改Connector描述.md)

## 使用限制

消息队列Kafka版对Connector的限制如下：

|项目|限制值|
|--|---|
|数量|3|
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

如果您需要将某个地域的Connector的数据同步到另一个地域的阿里云服务，您需要为该Connector开启公网访问，然后在公网进行数据同步。详情请参见[为Connector开启公网访问](/cn.zh-CN/用户指南/Connector/为Connector开启公网访问.md)。

