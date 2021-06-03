# Billing

This topic describes the instance editions, regions, instance types, billable items, and billing methods of Message Queue for Apache Kafka.

**Note:** The connector feature of Message Queue for Apache Kafka is in public preview. This feature is independent of Message Queue for Apache Kafka instances. Therefore, you are not charged on the Message Queue for Apache Kafka side when you use a connector to synchronize data between Message Queue for Apache Kafka and another Alibaba Cloud service. Alibaba Cloud does not provide service level agreement \(SLA\) commitments for the connector feature in public preview. For information about the SLA commitments and billing of the services that are related to the connector feature, see the documentation of the services.

## Instance specifications

The following table describes the specifications for each instance edition of Message Queue for Apache Kafka.

|Item|Standard Edition \(High Write\)|Professional Edition \(High Write\)|Professional Edition \(High Read\)|
|----|-------------------------------|-----------------------------------|----------------------------------|
|Version|Only version 0.10.x is supported.|-   Versions 0.10.x to 2.x are supported.
-   Version 2.x is compatible with versions 0.11.x and 1.x.
-   By default, version 0.10.x is deployed. For more information about how to upgrade the version, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

|-   Versions 0.10.x to 2.x are supported.
-   Version 2.x is compatible with versions 0.11.x and 1.x.
-   By default, version 0.10.x is deployed. For more information about how to upgrade the version, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md). |
|Ratio of maximum read traffic to maximum write traffic|1:1|1:1|5:1|
|Instance type|Virtual instance where specific resources are shared|Dedicated instance|Dedicated instance|
|Message retention period|Up to seven days|Customizable|Customizable|
|Disaster recovery|Single-zone deployment|Multi-zone deployment|Multi-zone deployment|
|Performance optimization|Not supported|Customizable|Customizable|
|Cross-region message router|Not supported|Not supported|Not supported|
|Topic|The number of topics that you can create is the same as the number of topics that you purchase.|The number of topics that you can create is twice the number of topics that you purchase.|The number of topics that you can create is twice the number of topics that you purchase.|

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

-   VPC type: Instances of this type can be accessed only from a virtual private cloud \(VPC\).
-   Internet and VPC type: Instances of this type can be accessed from the Internet and a VPC.

## Billable items

The following table describes the billable items for Message Queue for Apache Kafka.

|Billable item|Description|
|-------------|-----------|
|Public traffic|Public traffic is divided into read traffic and write traffic. The maximum read traffic and maximum write traffic provided by Message Queue for Apache Kafka are the same. Select a bandwidth based on your peak read or write traffic, whichever is higher. This billable item applies only to instances of the Internet and VPC type.|
|Traffic specification|-   The traffic specification refers to all the traffic consumed by your elastic network interfaces \(ENIs\), including business traffic and in-cluster replication traffic. Business traffic is the actual messaging traffic of your business. In-cluster replication traffic includes the traffic generated when the data in your Message Queue for Apache Kafka cluster is backed up multiple times. By default, the cluster has a total of three replicas after backup.
-   Business traffic is divided into read traffic and write traffic. The ratio of maximum read traffic to maximum write traffic is 1:1 for the Professional Edition \(High Write\) and is 5:1 for the Professional Edition \(High Read\). Select an ENI traffic specification based on your peak read or write traffic, whichever is higher. To ensure business stability, we recommend that you purchase a margin for buffering. The margin is about 30% of your peak read or write traffic, whichever is higher. |
|Disk Capacity|-   In consideration of performance and storage space, the minimum disk capacity varies based on the traffic specification.
-   Message Queue for Apache Kafka supports ultra disks and solid-state drives \(SSDs\). We recommend that you use SSDs.
-   The price of a disk varies with the disk type.
-   Exercise caution when you select a disk type, because the disk type cannot be changed after the order is placed.
-   By default, data is stored in three replicas.
    -   For a Standard Edition instance, if you purchase a disk of 300 GB in size, the actual storage space that you can use to store your business data is 100 GB. The remaining 200 GB is used to store backups.
    -   For a Professional Edition instance, if you purchase a disk of 300 GB in size, the actual storage space that you can use to store your business data is 300 GB. 600 GB of storage space is free for you to store backups.

**Note:** Free storage space applies only to topics whose storage engines are cloud storage. For more information about cloud storage, see [Storage engine comparison](/intl.en-US/Introduction/Storage engine comparison.md). |
|Topic specification|-   The maximum number of topics or partitions allowed varies with your traffic specification.
-   In addition to the default number of partitions, 16 partitions are added each time you purchase a topic. Assume that you have purchased an instance that has 50 topics, a maximum traffic volume of 20 MB/s, and 400 default partitions. After you purchase another 10 topics for this instance, 160 partitions are added to the instance. The total number of partitions increases to 560.
-   The number of topics that you can create on a Professional Edition instance is twice the number of topics that you purchase. For example, if you purchase a Professional Edition instance that has 50 topics, the number of topics that you can create on this instance is 100. |

**Note:**

-   You are charged for the items that are described in the previous tables.
-   You can adjust the value of the Message Retention Period parameter to save disk space. This parameter specifies the maximum period for which messages can be retained when disk space is sufficient. If disk usage reaches 90%, the disk capacity is insufficient. In this case, the system deletes messages from the earliest ones to ensure service availability. By default, messages are retained for a maximum of 72 hours. You can also select a period between 24 hours and 168 hours.
-   The number of API calls is not a billable item.

## Billing methods

Message Queue for Apache Kafka supports only the subscription billing method.

Billing formulas

Billing formulas are associated with instance types.

-   If you have purchased an instance of the Internet and VPC type, select the public traffic, traffic specification, disk capacity, and number of additional topics as required. The following formula is used to calculate billing fees:

    Total fees = \(Unit price of public traffic + Unit price of traffic specification + Unit price of disk capacity × Disk capacity to be purchased/100 + Price of one topic × Number of additional topics\) × Number of months

-   If you have purchased an instance of the VPC type, select the traffic specification, disk capacity, and number of additional topics as required. The following formula is used to calculate billing fees:

    Total fees = \(Unit price of traffic specification + Unit price of disk capacity × Disk capacity to be purchased/100 + Price of one topic × Number of additional topics\) × Number of months


Billing rules

Billing rules include the rules for the public traffic, traffic specifications, disk capacity, and additional topics:

-   Billing rules for public traffic

    For more information, see [Subscription](/intl.en-US/Pricing/Subscription.md).

-   Billing rules for traffic specifications

    The billing rules for traffic specifications vary with your instance edition.

    |Traffic specification|Maximum read traffic of ENIs \(MB/s\)|Maximum write traffic of ENIs \(MB/s\)|Topics in the subscription|Partitions in the subscription|Unit price in region group 1 \(USD/month\)|Unit price in region group 2 \(USD/month\)|Unit price in region group 3 \(USD/month\)|
    |---------------------|-------------------------------------|--------------------------------------|--------------------------|------------------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |alikafka.hw.2xlarge|3×20|3×20|50|400|250|370|340|
    |alikafka.hw.3xlarge|3×30|3×30|50|500|360|530|480|
    |alikafka.hw.6xlarge|3×60|3×60|80|600|520|760|700|
    |alikafka.hw.9xlarge|3×90|3×90|100|800|660|960|880|
    |alikafka.hw.12xlarge|3×120|3×120|150|900|800|1160|1070|

    |Traffic specification|Maximum read traffic of ENIs \(MB/s\)|Maximum write traffic of ENIs \(MB/s\)|Topics in the subscription|Partitions in the subscription|Unit price in region group 1 \(USD/month\)|Unit price in region group 2 \(USD/month\)|Unit price in region group 3 \(USD/month\)|
    |---------------------|-------------------------------------|--------------------------------------|--------------------------|------------------------------|------------------------------------------|------------------------------------------|------------------------------------------|
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

    |Traffic specification|Maximum read traffic of ENIs \(MB/s\)|Maximum write traffic of ENIs \(MB/s\)|Topics in the subscription|Partitions in the subscription|Unit price in region group 1 \(USD/month\)|Unit price in region group 2 \(USD/month\)|Unit price in region group 3 \(USD/month\)|
    |---------------------|-------------------------------------|--------------------------------------|--------------------------|------------------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |alikafka.hr.2xlarge|50+2×10|10+2×10|50|1100|600|870|800|
    |alikafka.hr.3xlarge|75+2×15|15+2×15|50|1200|780|1040|1040|
    |alikafka.hr.6xlarge|150+2×30|30+2×30|80|1400|1130|1510|1510|
    |alikafka.hr.9xlarge|180+2×45|45+2×45|100|1600|1440|1920|1920|
    |alikafka.hr.12xlarge|240+2×60|60+2×60|150|1800|1750|2330|2330|

-   Billing rules for disk capacity

    |Disk type|Disk capacity \(GB\)|Unit price in region group 1 \(USD/month\)|Unit price in region group 2 \(USD/month\)|Unit price in region group 3 \(USD/month\)|
    |---------|--------------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |Ultra disk|100|6|8|8|
    |SSD|100|16|23|21|

    **Note:** If the disk capacity in the subscription does not meet your requirements, you must purchase additional disk capacity by adjusting the value of Disk Capacity on the buy page to a larger value.

-   Billing rules for additional topics

    |Billable item|Number of topics|Unit price in region group 1 \(USD/month\)|Unit price in region group 2 \(USD/month\)|Unit price in region group 3 \(USD/month\)|
    |-------------|----------------|------------------------------------------|------------------------------------------|------------------------------------------|
    |Topic|1|7|11|10|

    **Note:** If the number of topics in the subscription does not meet your requirements, you must purchase additional topics by adjusting the value of Supported Topics on the buy page to a larger value.


