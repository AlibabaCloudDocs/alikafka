---
keyword: [kafka, topic, 自动创建]
---

# 自动创建Topic

自动创建Topic适用于测试场景或临时迁移场景。在生产环境长期开启自动创建Topic，可能会因为客户端使用不当而出现资源随意创建的问题。在生产环境，建议您尽可能通过消息队列Kafka版控制台或OpenAPI来管理Topic。

您已完成以下操作：

1.  确保消息队列Kafka版实例的大版本为2.X，小版本为最新版本。如何查看并升级消息队列Kafka版实例的大版本或小版本，请参见[升级实例版本](/cn.zh-CN/用户指南/实例/升级实例版本.md)。
2.  提交[工单](https://selfservice.console.aliyun.com/ticket/category/alikafka)为消息队列Kafka版实例开启自动创建Topic。

自动创建Topic是指当消息队列Kafka版实例开启自动创建Topic后，客户端向消息队列Kafka版实例发送获取不存在的Topic的元数据请求时，例如向不存在的Topic发送消息，消息队列Kafka版实例会自动创建该Topic。

## 通过Producer API或Consumer API自动创建Topic

**说明：**

-   自动创建的Topic的名称需遵循消息队列Kafka版的Topic命名规则，否则Topic不会被创建，Producer或Consumer会收到类似`获取不到Metadata`的错误。
-   自动创建的Topic的数量需遵循消息队列Kafka版实例的规格限制，否则Topic不会被创建，Producer或Consumer会收到类似`获取不到Metadata`的错误。
-   自动创建的Topic的分区总数需遵循消息队列Kafka版实例的规格限制，否则Topic不会被创建，Producer或Consumer会收到类似`获取不到Metadata`的错误。
-   开启自动创建Topic后，您需要及时关注消息队列Kafka版控制台上的Topic和分区的配额信息，以便增购新的资源，并删除无用的资源。
-   自动创建的Topic的存储引擎默认为云存储、分区数默认为12、备注默认为auto created by metadata。通过Producer API或者Consumer API自动创建Topic，本质上其实是发送获取Topic的Metadata的请求。如果消息队列Kafka版服务端发现请求的Topic不存在，就会自动创建该Topic。

为实例开启自动创建Topic后，您可以通过调用Producer API或Consumer API自动创建Topic。

-   调用Producer API自动创建Topic的示例代码如下：

    ```
    // 以Java API为例，其余语言的API类似。
    ProducerRecord<String, String> kafkaMessage =  new ProducerRecord<String, String>("newTopicName", value);
    // 如果该Topic不存在，则自动创建存储引擎默认为云储存、分区数默认为12的Topic。
    Future metadataFuture = producer.send(kafkaMessage);
    ```

-   调用Consumer API自动创建Topic的示例代码如下：

    ```
    // 以Java API为例，其余语言的API类似。
    consumer.subscribe(Collections.singletonList("newTopicName"));
    // 如果该Topic不存在，则自动创建存储引擎默认为云储存、分区数默认为12的Topic。
    consumer.poll(Duration.ofSeconds(1));
    ```


消息队列Kafka版控制台的**Topic管理**页面显示自动创建的Topic。

## 通过AdminClient.createTopics\(\)创建Topic

**说明：**

-   创建的Topic的名称需遵循消息队列Kafka版的Topic命名规则，否则Topic不会被创建。
-   创建的Topic的数量需遵循消息队列Kafka版实例的规格限制，否则Topic不会被创建，并抛出以下错误：`Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max topic count of your instance is xxx, topic runs out.`。
-   创建的Topic的分区总数需遵循消息队列Kafka版实例的规格限制，否则Topic不会被创建，并且抛出以下错误：`Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max partition count of your instance is xxx, partition runs out.`。
-   创建的单个Topic的分区数需小于或等于360，否则Topic不会被创建，并抛出以下错误：`Exception in thread "main" java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.InvalidTopicException: The max partition count of a single topic is 360.`。
-   必须在上一次创建Topic请求被处理完成后，提交新的创建Topic请求，否则Topic不会被创建，并抛出以下错误：`org.apache.kafka.common.errors.InvalidTopicException: Try to create topic: topicA. But some other topics are being created, please try again later.`
-   不支持通过assignments的方式来创建。
-   不支持创建时配置Topic名称、存储引擎、cleanup.policy、分区数以外的参数（例如min.isr），消息队列Kafka版会为您自动配置最优参数。
-   创建的Topic的备注默认为auto created by admin-client。如果配置了`"cleanup.policy":"compact"`，则存储引擎为Local存储，否则为云存储。
-   开启了ACL的消息队列Kafka版实例不支持调用`AdminClient.createTopics()`创建Topic。

为实例开启自动创建Topic后，您可以通过调用`AdminClient.createTopics()`方法创建Topic。

示例代码如下：

```
// 以Java API为例，其余语言的API类似。
CreateTopicsOptions createTopicsOptions = new CreateTopicsOptions();
// 建议20秒以上。
createTopicsOptions.timeoutMs(20000);
// 如果仅仅用于验证，可以使用下面这行代码，Topic不会被真实创建。
// createTopicsOptions.validateOnly(true);
Collection newTopics = new ArrayList<>();

// 创建云存储引擎Topic、分区数为12、replicationFactor默认为1的Topic。
NewTopic cloudTopic = new NewTopic("cloudTopic", 12, (short) 1);
newTopics.add(cloudTopic);

// 创建存储引擎为Local存储、分区数为12, replicationFactor默认为3的Topic。
NewTopic compactLocalTopic = new NewTopic("compactLocalTopic", 12, (short) 3);
Map<String, String> map = new HashMap<>();

// 配置cleanup.policy为compact。
map.put(TopicConfig.CLEANUP_POLICY_CONFIG, TopicConfig.CLEANUP_POLICY_COMPACT);
compactLocalTopic.configs(map);
newTopics.add(compactLocalTopic);

// 提交请求。
CreateTopicsResult result = adminClient.createTopics(newTopics, createTopicsOptions);

// 等待创建，创建成功后不会有任何报错；如果创建失败或者超时，这里将会报错。
result.all().get();
```

消息队列Kafka版控制台的**Topic管理**页面显示创建的Topic。

