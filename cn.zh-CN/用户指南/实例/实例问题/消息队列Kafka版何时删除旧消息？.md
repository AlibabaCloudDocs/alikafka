# 消息队列Kafka版何时删除旧消息？

为避免因磁盘容量不足而导致机器宕机，进而影响服务可用性，消息队列Kafka版对磁盘使用率进行了动态控制。

-   磁盘使用率低于85%：每天凌晨4点集中删除超过存储时间的消息。
-   磁盘使用率达到85%：立即清除超过存储时间的消息。
-   磁盘使用率达到90%：无论消息是否超过存储时间，按服务端存储消息的时间先后顺序清除消息。

**说明：** 一般情况下，为了保证您的业务健康性（拥有充足的消息回溯能力），建议您的磁盘使用率不要超过70%。

