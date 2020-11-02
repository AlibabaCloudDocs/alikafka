# How can I select the instance edition?

Message Queue for Apache Kafka provides the Standard Edition and Professional Edition. For more information, see [Instance editions](/intl.en-US/Pricing/Billing.md). You can select the instance edition based on the migration status.

-   Standard Edition
    -   Peak traffic = Total traffic in the cluster/3 \(For optimization\)
    -   Disk size = Average traffic × Storage duration × 3 \(replicas\)
    -   Number of topics: depends on the actual business demand.

        **Note:** We recommend that you optimize topics to reduce costs when migrating your data to the cloud.

-   Professional Edition
    -   Peak traffic = Total traffic in the cluster/3 \(For optimization\)
    -   Disk size = Average traffic × Storage duration × n \(replicas\)

        **Note:** When you create a topic, n is 1 for cloud storage and 3 for local storage. For more information about the comparison between cloud storage and local storage, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

    -   Number of topics: depends on the actual business demand.

        **Note:** We recommend that you optimize topics to reduce costs when migrating your data to the cloud.


