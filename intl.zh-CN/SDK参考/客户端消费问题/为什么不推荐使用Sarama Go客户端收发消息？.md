# 为什么不推荐使用Sarama Go客户端收发消息？

## 问题现象

Sarama Go客户端存在以下已知问题：

-   当Topic新增分区时，Sarama Go客户端无法感知并消费新增分区，需要客户端重启后，才能消费到新增分区。
-   当Sarama Go的客户端同时订阅超过两个以上的Topic时，有可能会导致部分分区无法正常消费消息。
-   当将消费位点策略设置为`Oldest(earliest)`的客户端宕机时，由于Sarama Go客户端自行实现”OutOfRange机制“，有可能会导致客户端从最小位点开始重新消费所有消息。

1.  建议尽早将Sarama Go客户端替换为Confluent Go客户端。

    Confluent Go客户端的Demo地址，请访问[kafka-confluent-go-demo](https://github.com/AliwareMQ/aliware-kafka-demos/tree/master/kafka-confluent-go-demo)。


