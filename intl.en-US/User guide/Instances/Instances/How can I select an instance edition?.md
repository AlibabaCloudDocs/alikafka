# How can I select an instance edition?

Message Queue for Apache Kafka provides two instance editions: Standard Edition and Professional Edition. You can select the instance edition based on your migration needs.

-   Standard Edition
    -   Traffic specification = Total traffic in the cluster/3 \(for optimization\)
    -   Disk capacity = Average traffic × Storage duration × 3 \(replicas\)
    -   Number of topics: depends on the actual business demand.

        **Note:** We recommend that you optimize topics to reduce costs when you migrate your data to the cloud.

-   Professional Edition
    -   Traffic specification = Total traffic in the cluster/3 \(for optimization\)
    -   Disk capacity = Average traffic × Storage duration × n \(replicas\)

        **Note:** When you create a topic, n is 1 for cloud storage and 3 for local storage. For more information about the comparison between cloud storage and local storage, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md).

    -   Number of topics: depends on the actual business demand.

        **Note:** We recommend that you optimize topics to reduce costs when you migrate your data to the cloud.


