# When are old messages deleted in Message Queue for Apache Kafka?

-   When disk usage is less than 85%, expired messages are deleted at 04:00 every day.
-   When disk usage reaches 85%, expired messages are deleted immediately.
-   When disk usage reaches 90%, old messages \(expired or not\) are deleted according to time.

Message Queue for Apache Kafka dynamically controls disk usage to prevent instance downtime due to insufficient disk space, which may affect service availability. We recommend that you keep disk usage at no more than 70% to ensure business health so that messages can be traced back. To resize disks, see [Upgrade the instance configuration](/intl.en-US/User guide/Instances/Upgrade the instance configuration.md).

