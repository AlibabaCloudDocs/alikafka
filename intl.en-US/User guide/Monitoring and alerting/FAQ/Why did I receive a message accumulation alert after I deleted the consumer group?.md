# Why did I receive a message accumulation alert after I deleted the consumer group?

The consumer group is logically deleted from the console. However, the information such as the consumer offset on the broker is not deleted. The accumulation alert is handled based on the consumer offset. Therefore, you still receive the accumulation alert.

If you do not want to receive the accumulation alert after deleting the consumer group, you can perform the following operations:

-   Disable the accumulation alert.

-   Wait until the consumer offset expires.

    The Message Queue for Apache Kafka consumer offset is stored in an internal topic and cannot be directly deleted. If the consumer offset is not updated after the message retention period ends, it will be cleared due to expiration.

    **Note:** Earlier users who have not enabled the clearing mechanism need to upgrade the broker to the latest version on the **Instance Details** page.


