---
keyword: [kafka, filebeat]
---

# 接入Filebeat

本文介绍如何将消息队列Kafka版接入Filebeat。

## Filebeat

Filebeat是用于转发和集中日志数据的轻量级传输程序。Filebeat可以监听指定的日志文件或位置，从中收集日志事件并将其转发到Elasticsearch或Logstash进行索引。Filebeat的工作原理如下：

1.  Filebeat启动一个或多个Input，Input在指定的位置中查找日志数据。
2.  Filebeat为每个找到的日志启动Harvester，Harvester读取日志并将日志数据发送到libbeat。
3.  libbeat聚集数据，然后将聚集的数据发送到配置的Output。

## 接入优势

消息队列Kafka版接入Filebeat可以带来以下优势：

-   异步处理：防止突发流量。
-   应用解耦：当下游异常时，不会影响上游工作。
-   减少开销：减少Filebeat的资源开销。

## 接入方案

消息队列Kafka版支持以下方式接入Filebeat：

-   VPC
    -   [作为Input接入](/intl.zh-CN/生态对接/开源生态/Filebeat/VPC/作为Input接入.md)
    -   [作为Output接入](/intl.zh-CN/生态对接/开源生态/Filebeat/VPC/作为Output接入.md)
-   公网
    -   [作为Input接入]()
    -   [作为Output接入]()

