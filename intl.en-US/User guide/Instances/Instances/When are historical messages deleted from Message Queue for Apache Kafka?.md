# When are historical messages deleted from Message Queue for Apache Kafka?

Instance downtime may occur due to insufficient disk capacity. To avoid instance downtime and maintain service availability, Message Queue for Apache Kafka dynamically controls disk usage of each instance.

-   If the disk usage is lower than 85%, expired messages are deleted at 04:00 every day.
-   If the disk usage reaches 85%, expired messages are immediately deleted.
-   If the disk usage reaches 90%, stored messages \(expired or not\) are deleted in the sequence that they were stored in the clients.

**Note:** To ensure your service reliability and message backtracking capabilities, we recommend that you maintain a disk usage of lower than 70%.

