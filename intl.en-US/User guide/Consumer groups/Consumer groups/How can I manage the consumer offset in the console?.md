---
keyword: [kafka, consumer, offset]
---

# How can I manage the consumer offset in the console?

This topic describes how to manage the consumer offset when a consumer stops reading messages due to an exception.

The consumer offset is not necessarily committed after a message is consumed. The broker records the consumer offset committed by the consumer.

The committing mechanism depends on the client SDK you use. Generally, the following two mechanisms are supported:

-   Automatic committing: The SDK commits the consumer offset of the latest consumed message plus 1 at an interval.
-   Manual committing: The consumer offset of the latest consumed message plus 1 is committed in the application.

Log on to the console and go to the Consumer Groups page. Then, you can click **Consumption Status** to view the latest consumer offset committed. The consumer continues the consumption from this consumer offset. For more information, see [View the consumption status](/intl.en-US/User guide/Consumer groups/View the consumption status.md).

You can manually change the consumer offset recorded by the broker in the console. You can move it backward for repeated consumption or move it forward to skip consumption of specified messages.

**Note:** To [Reset consumer offsets](/intl.en-US/User guide/Consumer groups/Reset consumer offsets.md) in the console, you need to stop the consumer client first. Otherwise, the reset result may be overwritten by that of the consumer.

