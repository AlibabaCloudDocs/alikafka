# When are historical messages deleted from Message Queue for Apache Kafka?

-   When disk usage is less than 85%, messages that expired are deleted at 04:00 every day.
-   When disk usage reaches 85%, messages that expired are immediately deleted.
-   When disk usage reaches 90%, deletion starts from the earliest stored messages no matter whether they are expired or not.

Message Queue for Apache Kafka dynamically controls disk usage to prevent instance downtime due to insufficient disk space and ensure service availability. We recommend that you keep disk usage at no more than 70% to ensure business health so that messages can be traced back. To resize disks, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).

