# What problems may occur if you upgrade Message Queue for Apache Kafka brokers?

If you upgrade Message Queue for Apache Kafka brokers, messages may be out of order, clients may be disconnected from the brokers, or messages cannot be evenly distributed to partitions.

If you upgrade Message Queue for Apache Kafka brokers, the following problems may occur:

-   During the upgrade, all brokers in the Message Queue for Apache Kafka cluster are restarted one by one. Services are not interrupted during the restart of brokers. However, the messages consumed within 5 minutes after each broker is restarted may be out of order in specific partitions.
-   During the restart of brokers, clients that are connected to the brokers may be disconnected. If the clients support automatic reconnection, other brokers automatically take over the services.
-   During the upgrade and restart of brokers, the volumes of messages processed by different partitions are also uneven. You need to evaluate the impacts of the upgrade on your business.

It takes about 5 to 15 minutes to upgrade all the brokers. If you have multiple instances, you can first upgrade a test cluster. After you upgrade the test cluster and verify the upgrade results, you can upgrade the production cluster.

**Note:** If you use a Go client developed with the Sarama library to send and subscribe to messages, messages may be repeatedly consumed when you upgrade brokers. For more information, see [Why is it not recommended to use a Go client developed with the Sarama library to send and subscribe to messages?](/intl.en-US/SDK reference/Consumption on clients/Why is it not recommended to use a Go client developed with the Sarama library to send and subscribe to messages?.md).

