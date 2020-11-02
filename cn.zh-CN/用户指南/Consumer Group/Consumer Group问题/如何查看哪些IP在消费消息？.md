---
keyword: [kafka, 消费, ip]
---

# 如何查看哪些IP在消费消息？

在控制台根据Consumer Group查看消费状态。

在控制台根据Consumer Group查看消费状态，单击**详情**，可以看到各个分区的**owner**，进而看到对应的IP。

如果看到**owner**为空，说明客户端不在线或者处于**Rebalance**中。

