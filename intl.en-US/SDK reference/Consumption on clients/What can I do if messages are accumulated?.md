---
keyword: [kafka, stack information, message accumulation]
---

# What can I do if messages are accumulated?

We recommend that you view the stack information to troubleshoot message accumulation. In most cases, message accumulation is caused by slow consumption or blocked consumption threads.

The consumer actively pulls Message Queue for Apache Kafka messages from the broker. Generally, message pulling from the broker does not cause bottlenecks against consumption as the consumer is allowed to pull multiple messages at a time.

In most cases, message accumulation is caused by slow consumption or blocked consumption threads.

We recommend that you query the time consumed by messages, or view the stack information to troubleshoot message accumulation.

**Note:** You can run the jstack command to query the stack information for Java threads.

