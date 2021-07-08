# 为什么不推荐使用Sarama Go客户端收发消息？

## 问题现象

Sarama Go客户端存在以下已知问题：

-   当Topic新增分区时，Sarama Go客户端无法感知并消费新增分区，需要客户端重启后，才能消费到新增分区。
-   当Sarama Go客户端同时订阅两个以上的Topic时，有可能会导致部分分区无法正常消费消息。
-   当Sarama Go客户端的消费位点重置策略设置为`Oldest(earliest)`时，如果客户端宕机或服务端版本升级，由于Sarama Go客户端自行实现OutOfRange机制，有可能会导致客户端从最小位点开始重新消费所有消息。

1.  建议尽早将Sarama Go客户端替换为Confluent Go客户端。

    Confluent Go客户端的Demo地址，请访问[kafka-confluent-go-demo](https://github.com/AliwareMQ/aliware-kafka-demos/tree/master/kafka-confluent-go-demo)。

    **说明：** 如果无法在短期内替换客户端，请注意以下事项：

    -   针对生产环境，请将位点重置策略设置为`Newest(latest)`；针对测试环境，或者其他明确可以接收大量重复消息的场景，设置为`Oldest(earliest)`。
    -   如果发生了位点重置，产生大量堆积，您可以使用消息队列Kafka版控制台提供的重置消费位点功能，手动重置消费位点到某一时间点，无需改代码或换Consumer Group。具体操作，请参见[重置消费位点](/intl.zh-CN/控制台操作/Consumer Group/重置消费位点.md)。

