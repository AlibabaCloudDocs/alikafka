---
keyword: [kafka, message accumulation reported by an alert, message accumulation displayed in the console, inconsistency]
---

# Why is the number of accumulated messages reported by an accumulation alert different from that displayed in the console?

In most cases, this issue is caused by dead partitions.

In most cases, this issue is caused by dead partitions. Assume that a partition has no offset recorded on the broker. In this case, by default, the Message Queue for Apache Kafka console calculates the number of accumulated messages by subtracting the minimum offset from the maximum offset. However, an accumulation alert takes the maximum offset as the number of accumulated messages for a dead partition by default. If your instance is in the latest minor version, an accumulation alert calculates the number of accumulated messages in the same way as the Message Queue for Apache Kafka console. Therefore, if you find any inconsistency between the number of accumulated messages reported by an accumulation alert and that displayed in the Message Queue for Apache Kafka console, we recommend that you upgrade the minor version of your instance to the latest version. For more information, see [Upgrade the minor version of an instance](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

