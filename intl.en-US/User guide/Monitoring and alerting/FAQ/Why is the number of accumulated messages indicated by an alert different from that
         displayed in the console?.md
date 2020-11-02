# Why is the number of accumulated messages indicated by an alert different from that displayed in the console?

Generally, this is due to dead partitions. When a partition has no offset recorded on the broker, by default, the console calculates the number of accumulated messages by subtracting the maximum offset by the minimum offset. However, the accumulation alert takes the maximum offset as the number of accumulated messages for a dead partition by default. Currently, this problem is solved for the broker in a new version to be released, where the number of accumulated messages is calculated by subtracting the maximum offset by the minimum offset.

