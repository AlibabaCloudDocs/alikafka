# 消息队列 Kafka 搭配云 HBase 和 Spark 构建一体化数据处理平台 {#concept_121370_zh .concept}

云 HBase X-Pack 是基于 Apache HBase、Phoenix、Spark 深度扩展，融合 Solr 检索等技术，支持海量数据的一站式存储、检索与分析。融合云 Kafka + 云 HBase X-Pack 能够构建一体化的数据处理平台，支持风控、推荐、检索、画像、社交、物联网、时空、表单查询、离线数仓等场景，助力企业数据智能化。

下图是业界广泛应用的大数据中台架构，其中 HBase 和 Spark 选择云 HBase X-Pack。产品详情请参见 [X-pack Spark 分析引擎](https://help.aliyun.com/document_detail/93899.html?spm=a2c4g.11186623.2.10.3bf81f630K08Hz)[立即购买\>\>](https://hbase.console.aliyun.com/hbase/cn-shenzhen/clusters)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/998865/156697677353226_zh-CN.png)

-   消息流入：Flume、Logstash 或者在线库的 Binlog 流入消息中间件 Kafka。

-   实时计算：通过 X-Pack Spark Streaming 实时的消费 Kafka 的消息，写入到云 HBase 中对外提供在线查询。

-   实时存储与检索：云 HBase 融合 Solr 以及 Phoenix SQL 层能够提供海量的实时存储，以及在线查询检索。

-   批处理、数仓及算法：在线存储 HBase 的数据可以自动归档到 X-Pack Spark 数仓。全量数据沉淀到 Spark 数仓（HiveMeta），做批处理、算法分析等复杂计算，结果回流到在线库对外提供查询。


该套方案的实践操作请参见 [Spark 对接 Kafka 快速入门](https://help.aliyun.com/document_detail/114567.html?spm=a2c4g.11186623.2.13.3bf81f630K08Hz)。同时，有云 HBase 和 Spark 的示例代码请参见[Demo](https://github.com/aliyun/aliyun-apsaradb-hbase-demo/tree/master/spark)。

