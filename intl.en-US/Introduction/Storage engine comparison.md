---
keyword: [topic, Kafka, storage engine]
---

# Storage engine comparison

Message Queue for Apache Kafka supports two storage engines: cloud storage and local storage. This topic compares the two storage engines to help you select one based on your business requirements.

**Note:**

-   For more information about the open source versions of different instance specifications, see [Instance editions](/intl.en-US/Pricing/Billing.md).
-   You can select a storage engine when you create a topic for an instance of the Professional Edition. The storage engine can be local storage or cloud storage. You cannot select a storage engine when you create a topic for an instance of the Standard Edition. Cloud storage is used as the storage engine by default. For more information about how to select a storage engine for a topic, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).
-   Cloud storage has the benefits of the underlying storage of Alibaba Cloud. Compared with local storage, cloud storage provides better performance in auto-scaling, reliability, availability, and cost-effectiveness. Therefore, we recommended that you use cloud storage in most cases.
-   If you have special requirements, such as Compact, idempotence, transaction, and partitionally ordered messages, we recommend that you use local storage. These scenarios are rare.
-   Local storage refers to the use of the native ISR algorithm, not local disks.

