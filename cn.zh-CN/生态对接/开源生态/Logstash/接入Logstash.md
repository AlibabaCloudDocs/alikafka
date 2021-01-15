---
keyword: [kafka, logstash]
---

# 接入Logstash

本文介绍如何将消息队列Kafka版接入Logstash。

## Logstash

Logstash是开源的服务器端数据处理管道，能够同时从多个数据源采集数据，然后对数据进行转换，并将数据写入指定的存储中。Logstash的数据处理流程如下：

1.  输入：采集各种格式、大小和来源的数据。在实际业务中，数据往往以各种各样的形式分散或集中地存储在多个系统中，Logstash支持多种数据输入方式，可以在同一时间从多种数据源采集数据。Logstash能够以连续的流式传输方式从日志、Web应用、数据存储等采集数据。
2.  过滤：实时解析和转换数据。数据从源传输到目标存储的过程中，Logstash过滤器能够解析各个事件，识别已命名的字段来构建结构，并将它们转换成通用格式，通过更轻松、快速的方式分析数据来实现商业价值。
3.  输出：导出数据。Logstash提供多种数据输出方向，灵活解锁众多下游用例。

更多关于Logstash的介绍，请参见[Logstash简介](https://www.elastic.co/guide/en/logstash/current/introduction.html)。

## 接入优势

消息队列Kafka版接入Logstash可以带来以下优势：

-   异步处理：提高运行效率，防止突发流量影响用户体验。
-   应用解耦：当应用上下游中有一方存在异常情况，另一方仍能正常运行。
-   减少开销：减少Logstash的资源开销。

## 接入方案

消息队列Kafka版支持以下方式接入Logstash：

-   VPC
    -   [作为Input接入](/cn.zh-CN/生态对接/开源生态/Logstash/VPC/作为Input接入.md)
    -   [作为Output接入](/cn.zh-CN/生态对接/开源生态/Logstash/VPC/作为Output接入.md)
-   公网
    -   [作为Input接入](/cn.zh-CN/生态对接/开源生态/Logstash/公网/作为Input接入.md)
    -   [作为Output接入](/cn.zh-CN/生态对接/开源生态/Logstash/公网/作为Output接入.md)

