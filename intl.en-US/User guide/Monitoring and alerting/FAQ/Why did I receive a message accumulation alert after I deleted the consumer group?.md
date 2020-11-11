---
keyword: [consumergroup, accumulation alert, delete]
---

# Why did I receive a message accumulation alert after I deleted the consumer group?

After a consumer group is deleted, its information such as the consumer offset will not be deleted from the broker.

The consumer group is logically deleted from the console. However, its information such as the consumer offset is not deleted from the broker. The accumulation alert is handled based on the consumer offset. Therefore, you still received the accumulation alert.

If you do not want to receive the accumulation alert after you delete the consumer group, you can choose to:

-   Disable the accumulation alert.
-   Wait until the consumer offset expires.

    The Message Queue for Apache Kafka consumer offset is stored in an internal topic and cannot be directly deleted. If the consumer offset is not updated after the message retention period ends, it will be cleared due to expiration.

    **Note:** Earlier users who have not enabled the clearing mechanism need to upgrade the broker to the latest version on the **Instance Details** page.


