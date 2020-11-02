# What is impact of upgrading brokers?

Upgrading brokers has the following impact:

-   During the upgrade process, all brokers in the Message Queue for Apache Kafka cluster are restarted sequentially. The service is not interrupted when the brokers are restarted. However, the messages consumed within 5 minutes after each broker is restarted may be out of order in the specific partition. In particular, ordered messages will not be out of order, but will be temporarily unavailable.

-   Existing client connections may be interrupted in the restart process. Your clients must be able to automatically reconnect to other brokers that automatically take over the service.

-   During the upgrade and restart of the brokers, the volumes of messages processed by each partition are also uneven. You need to evaluate the impact of the upgrade on your business.


It takes about 5 to 15 minutes to upgrade all the brokers. If you have multiple instances, you can upgrade a test cluster first, and upgrade the production cluster after the test cluster is upgraded.

