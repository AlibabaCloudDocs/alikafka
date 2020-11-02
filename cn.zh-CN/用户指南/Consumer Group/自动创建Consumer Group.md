---
keyword: [kafka, consumer, 自动创建]
---

# 自动创建Consumer Group

自动创建Consumer Group适用于测试场景或临时迁移场景。在生产环境长期开启自动创建Consumer Group，可能会因为客户端使用不当而出现资源随意创建的问题。在生产环境，建议您尽可能通过消息队列Kafka版控制台或OpenAPI来管理Consumer Group。

您已完成以下操作：

1.  确保消息队列Kafka版实例的大版本为2.X，小版本为最新版本。如何查看并升级消息队列Kafka版实例的大版本或小版本，请参见[升级实例版本](/cn.zh-CN/用户指南/实例/升级实例版本.md)。
2.  提交[工单](https://selfservice.console.aliyun.com/ticket/category/alikafka)为消息队列Kafka版实例开启自动创建Consumer Group。

自动创建Consumer Group是指当消息队列Kafka版实例开启自动创建Consumer Group后，客户端向消息队列Kafka版实例发送获取不存在的Consumer Group的元数据请求时，例如使用不存在的Consumer Group订阅消息，消息队列Kafka版实例会自动创建该Consumer Group。

## 通过Consumer API自动创建Consumer Group

为消息队列Kafka版实例开启自动创建Consumer Group后，您可以通过调用Consumer API来自动创建Consumer Group。

**说明：**

-   自动创建的Consumer Group的名称需遵循消息队列Kafka版的Consumer Group命名规则，否则Consumer Group不会被创建。
-   自动创建的Consumer Group的数量需遵循消息队列Kafka版实例的规格限制，否则Consumer Group不会被创建。
-   自动创建的Consumer Group不受消息队列Kafka版控制台管控，因此消息队列Kafka版控制台的**Consumer Group管理**页面不会显示自动创建的Consumer Group。

1.  调用Consumer API自动创建Consumer Group。

    调用Consumer API自动创建Consumer Group的示例代码如下：

    ```
    props.put(ConsumerConfig.GROUP_ID_CONFIG, "newConsumerGroup");
    consumer.subscribe(Collections.singletonList("newTopicName"));
    // 如果该Consumer Group不存在，则自动创建该Consumer Group。
    consumer.poll(Duration.ofSeconds(1));
    ```


