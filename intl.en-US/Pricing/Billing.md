# Billing

This topic describes the instance editions, regions, instance types, billable items, and billing methods of Message Queue for Apache Kafka.

## Instance editions

The following table describes the specifications for each instance edition of Message Queue for Apache Kafka.

|Item|Standard Edition \(High Write\)|Professional Edition \(High Write\)|Professional Edition \(High Read\)|
|----|-------------------------------|-----------------------------------|----------------------------------|
|Version|Only version 0.10.x is supported.|-   Versions 0.10.x to 2.x are supported.
-   Version 2.x is compatible with versions 0.11.x and 1.x.
-   By default, version 0.10.x is deployed. For information about how to upgrade the version, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

|-   Versions 0.10.x to 2.x are supported.
-   Version 2.x is compatible with versions 0.11.x and 1.x.
-   By default, version 0.10.x is deployed. For information about how to upgrade the version, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md). |
|Ratio of peak read traffic to peak write traffic|1:1|1:1|5:1|
|Instance type|Virtual instance \(only part of virtual instances can be shared\)|Dedicated instance|Dedicated instance|
|Message retention period|Up to 7 days|Customizable|Customizable|
|Disaster recovery|Single-zone deployment|Multi-zone deployment|Multi-zone deployment|
|Performance optimization|Not supported|Customizable|Customizable|
|Cross-region message router|Not supported|Not supported|Not supported|
|Topic|The number of available topics is equal to the number of purchased topics.|The number of available topics is twice the number of purchased topics.|The number of available topics is twice the number of purchased topics.|

## Regions

The following table describes the regions where Message Queue for Apache Kafka can be deployed.

|Region group|Region|
|------------|------|
|Region group 1|China \(Hangzhou\)|
|China \(Shanghai\)|
|China \(Qingdao\)|
|China \(Beijing\)|
|China \(Zhangjiakou\)|
|China \(Hohhot\)|
|China \(Shenzhen\)|
|China \(Heyuan\)|
|China \(Chengdu\)|
|Region group 2|China \(Hong Kong\)|
|Singapore \(Singapore\)|
|Japan \(Tokyo\)|
|US \(Virginia\)|
|US \(Silicon Valley\)|
|Germany \(Frankfurt\)|
|UK \(London\)|
|Region group 3|Malaysia \(Kuala Lumpur\)|
|India \(Mumbai\)|
|Indonesia \(Jakarta\)|

## Instance types

Message Queue for Apache Kafka provides the following instance types:

VPC type: These instances can be accessed only by using a virtual private cloud \(VPC\).

## Billable items

The following table describes the billable items for Message Queue for Apache Kafka.

|Item|Description|
|----|-----------|
|Traffic specification|-   Traffic specifications involve only business traffic, instead of cluster traffic. Business traffic is the actual messaging traffic of your business. Cluster traffic includes business and replication traffic generated when your Message Queue for Apache Kafka instance in the Message Queue for Apache Kafka cluster is replicated three times by default.
-   Both read traffic and write traffic are calculated in business traffic. The ratio of peak read traffic to peak write traffic is 1:1 for the High Write edition and is 5:1 for the High Read edition. Select a traffic specification based on the maximum read or write traffic during peak hours. To ensure business stability, a margin is purchased for buffering. The margin is about 30% of the maximum read or write traffic during peak hours. |
|Disk capacity|-   The minimum disk capacity depends on your traffic specification and storage space.
-   Message Queue for Apache Kafka supports ultra disks and solid-state drives \(SSDs\). We recommend that you use SSDs.
-   The unit price is different for each disk type.
-   Exercise caution when you select a disk type. The disk type cannot be changed after the order is placed.
-   By default, data is stored in three replicas.
    -   For a Standard Edition instance, if you purchase a disk of 300 GB in size, the actual storage space you can use for your business is 100 GB and the remaining 200 GB of storage space is used to store backups.
    -   For a Professional Edition instance, if you purchase a disk of 300 GB in size, the actual storage space you can use for your business is 300 GB and the other two replicas for your business data are stored free of charge.

**Note:** Only topics whose storage engines are cloud storage support free storage for backups. For more information about cloud storage, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md). |
|Topic specification|-   The maximum number of topics or partitions depends on your traffic specification.
-   In addition to the default number of partitions, 16 partitions are added for each additional topic. For example, you have purchased an instance with 50 topics, 20 MB/s of bandwidth, and 400 default partitions. If you purchase another 10 topics for this instance, 160 additional partitions are added to the instance. The total number of partitions becomes 560.
-   The number of available topics for a Professional Edition instance is twice the number purchased. For example, if you purchase a Professional Edition instance with 50 topics, the actual number of topics you can create in the instance is 100. |

**Note:**

-   You are charged for the items described in the previous tables.
-   You can set Retain Messages For to save disk space. This parameter defines how long messages are retained when disk space is sufficient. If 90% of disk space is used, earlier messages are deleted to ensure service availability. By default, messages are retained for a maximum of 72 hours. You can change this value to a custom period between 24 and 168 hours.
-   The number of API calls is not a billable item.

## Billing methods

Message Queue for Apache Kafka supports only the subscription billing method.

Billing formulas

Total fees = \(Unit price of traffic specification + Unit price of disk capacity × Disk capacity/100 + Unit price of topic × Number of additional topics\) × Number of months

Billing rules

Billing rules include the rules for traffic specifications, disk capacity, and additional topics:

-   Billing rules for traffic specifications

    The billing rules for traffic specifications depend on the edition of the instance.

    |Traffic specification|Peak read traffic \(MB/s\)|Peak write traffic \(MB/s\)|Number of topics|Number of partitions|Unit price in Region group 1 \(USD/month\)|Unit price in Region group 2 \(USD/month\)|Unit price in Region group 3 \(USD/month\)|
    |---------------------|--------------------------|---------------------------|----------------|--------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |alikafka.hw.2xlarge|3×20|3×20|50|400|250|370|340|
    |alikafka.hw.3xlarge|3×30|3×30|50|500|360|530|480|
    |alikafka.hw.6xlarge|3×60|3×60|80|600|520|760|700|
    |alikafka.hw.9xlarge|3×90|3×90|100|800|660|960|880|
    |alikafka.hw.12xlarge|3×120|3×120|150|900|800|1160|1070|

    |Traffic specification|Peak read traffic \(MB/s\)|Peak write traffic \(MB/s\)|Number of topics|Number of partitions|Unit price in Region group 1 \(USD/month\)|Unit price in Region group 2 \(USD/month\)|Unit price in Region group 3 \(USD/month\)|
    |---------------------|--------------------------|---------------------------|----------------|--------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |alikafka.hw.2xlarge|3×20|3×20|50|1100|600|870|800|
    |alikafka.hw.3xlarge|3×30|3×30|50|1200|780|1040|1040|
    |alikafka.hw.6xlarge|3×60|3×60|80|1400|1130|1510|1510|
    |alikafka.hw.9xlarge|3×90|3×90|100|1600|1440|1920|1920|
    |alikafka.hw.12xlarge|3×120|3×120|150|1800|1750|2330|2330|
    |alikafka.hw.16xlarge|3×160|3×160|180|2000|2060|2740|2740|
    |alikafka.hw.20xlarge|3×200|3×200|200|2200|2980|3970|3970|
    |alikafka.hw.25xlarge|3×250|3×250|250|2500|3430|4570|4570|
    |alikafka.hw.30xlarge|3×300|3×300|300|3000|3880|5170|5170|
    |alikafka.hw.60xlarge|3×600|3×600|450|4500|5280|7030|7030|
    |alikafka.hw.80xlarge|3×800|3×800|500|5000|6220|8280|8280|
    |alikafka.hw.100xlarge|3×1000|3×1000|600|6000|7270|9670|9670|
    |alikafka.hw.120xlarge|3×1200|3×1200|700|7000|8100|10780|10780|
    |alikafka.hw.150xlarge|3×1500|3×1500|800|8000|9500|12640|12640|
    |alikafka.hw.180xlarge|3×1800|3×1800|900|9000|10910|14520|14520|
    |alikafka.hw.200xlarge|3×2000|3×2000|1000|10000|12070|16060|16060|

    |Traffic specification|Peak read traffic \(MB/s\)|Peak write traffic \(MB/s\)|Number of topics|Number of partitions|Unit price in Region group 1 \(USD/month\)|Unit price in Region group 2 \(USD/month\)|Unit price in Region group 3 \(USD/month\)|
    |---------------------|--------------------------|---------------------------|----------------|--------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |alikafka.hr.2xlarge|50+2×10|10+2×10|50|1100|600|870|800|
    |alikafka.hr.3xlarge|75+2×15|15+2×15|50|1200|780|1040|1040|
    |alikafka.hr.6xlarge|150+2×30|30+2×30|80|1400|1130|1510|1510|

-   Billing rules for disk capacity

    |Disk type|Disk capacity \(GB\)|Unit price in Region group 1 \(USD/month\)|Unit price in Region group 2 \(USD/month\)|Unit price in Region group 3 \(USD/month\)|
    |---------|--------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |Ultra disk|100|6|8|8|
    |SSD|100|16|23|21|

-   Billing rules for additional topics

    |Item|Number of topics|Unit price in Region group 1 \(USD/month\)|Unit price in Region group 2 \(USD/month\)|Unit price in Region group 3 \(USD/month\)|
    |----|----------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |Topic|1|7|11|10|


