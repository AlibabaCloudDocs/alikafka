---
keyword: [kafka, message accumulation indicated by an alert, message accumulation displayed in the console, different]
---

# Why is the number of accumulated messages reported in an alert different from that displayed in the console?

In most cases, this issue is caused by dead partitions.

In most cases, this issue is caused by dead partitions. When a partition has no offset recorded on the broker, by default, the console calculates the number of accumulated messages by subtracting the maximum offset by the minimum offset. However, the accumulation alert takes the maximum offset as the number of accumulated messages for a dead partition by default. This issue will be resolved in future releases, where the number of accumulated messages is calculated by subtracting the maximum offset by the minimum offset.

