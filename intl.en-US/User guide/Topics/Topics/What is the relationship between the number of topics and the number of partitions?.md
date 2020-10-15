# What is the relationship between the number of topics and the number of partitions?

One topic corresponds to 16 partitions.

In addition to the default number of partitions, 16 partitions are added for each additional topic. For example, assume that you purchased an instance with 50 topics, 20 Mbit/s traffic specification, and 400 partitions by default. After you add 10 topics, 160 partitions are added to this instance, and the total number of partitions becomes 560.

