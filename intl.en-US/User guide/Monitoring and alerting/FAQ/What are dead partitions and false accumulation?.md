---
keyword: [kafka, dead partition, false accumulation]
---

# What are dead partitions and false accumulation?

A dead partition is a partition to which no data has been sent for a long time. When dead partitions exist, the system always shows that messages are accumulated.

A dead partition is a partition to which no data has been sent for a long time. Dead partitions do not affect usage but interfere with monitoring and alerts. When dead partitions exist, the system always shows that messages are accumulated. The reason is that no data has been sent to dead partitions for a long time. The consumer no longer commits the offset, which expires after the retention period ends. When a partition has no offset recorded on the broker, by default, the maximum number of accumulated messages is calculated by subtracting the maximum offset by the minimum offset. False accumulation is reported when a dead partition still contains messages.

When a large number of messages are accumulated, these messages are deleted upon expiration. When the number of accumulated messages is small, it may take a long time to delete these messages because the broker retains at least a 1 GB segment.

The solution is to send messages as evenly as possible and make sure that each partition has data. If the amount of service data is small, regularly send heartbeat data to each partition.

