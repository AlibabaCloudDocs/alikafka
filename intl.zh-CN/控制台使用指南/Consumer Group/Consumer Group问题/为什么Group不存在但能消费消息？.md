# 为什么Group不存在但能消费消息？

## Condition

我在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)上，未查看到对应的Group，但此Group下却有消费线程在消费消息。

## Cause

-   如果客户端使用`assign`方式消费消息，那么即使不创建Group，可能也能消费消息。
-   如果客户端使用`subscribe`方式消费消息，删除Group后，消费线程未停止或者未发生Rebalance，那么消费线程还可以继续进行正常消费。

## Remedy

-   如果客户端使用`assign`方式消费消息，请提前在消息队列Kafka版控制台创建Group。

    请尽量复用Group，避免创建过多的Group而影响集群的稳定性。Group的数量限制，请参见[使用限制](/intl.zh-CN/产品简介/使用限制.md)。

-   在删除Group前，请确保已停止该Group下的所有消费线程。

    **说明：** 如果收到关于不存在的Group的消息堆积告警，详细的处理方法，请参见[删除Group后仍然能收到消息堆积的告警信息](~~157562~~)。


