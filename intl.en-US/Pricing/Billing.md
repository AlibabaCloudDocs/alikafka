# Billing

This topic describes the instance editions, network types, billing items, and billing methods of Message Queue for Apache Kafka for you to better understand the billing rules and select instances as needed.

**Note:** Message Queue for Apache Kafka is free of charge during public preview. You will be charged after public preview.

## Instance editions

Message Queue for Apache Kafka provides instances of the Professional Edition and the Standard Edition by specifications.

|Item|Professional Edition|Standard Edition|
|----|--------------------|----------------|
|Version|-   Version 0.10.x to version 2.x are supported.
-   Version 2.x is compatible with version 0.11.x and version 1.x.
-   The default version is version 0.10.x. For more information about how to upgrade the version, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

|Only version 0.10.x is supported.|
|Instance type|Exclusive instances, with a dedicated physical node for each instance|Virtual instances, with some instances sharing a physical node|
|Message storage time|Customized based on your scenarios|Up to 7 days|
|Disaster recovery|Multi-zone deployment|Single-zone deployment|
|Performance optimization|Customized based on business scenarios|Not supported|
|Cross-region message router|Coming soon|Not supported|
|Topic|Number of available topics = Number of purchased topics × 2|Number of available topics = Number of purchased topics|

## Network types

Instances of the VPC type: provide only the default endpoints and can be accessed only through [VPC](/intl.en-US/Product Introduction/What is a VPC?.md) networks.

## Billing items

The following table describes the billing items of Message Queue for Apache Kafka.

|Billing item|Description|Remarks|
|------------|-----------|-------|
|Peak traffic|The purchase of peak traffic is described as follows: -   Dual-channel read/write is implemented, and the read traffic is consistent with the write traffic. Set the peak traffic based on the maximum value of the peak read or write traffic. For business stability, a margin of about 30% of the maximum value of the peak read or write traffic is purchased for buffering. For example, if the peak write traffic is 100 MB/s and the peak read traffic is 200 MB/s, you need to purchase an instance that has the peak traffic as follows: 200 MB/s × \(1 + 30%\).
-   The maximum numbers of topics and partitions are specified for different peak traffic specifications. If the number of topics or partitions you want to create exceeds the maximum number specified for the peak traffic, you need to upgrade instance specifications or delete useless topics or partitions. For more information about how to upgrade instance specifications, see [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md).
-   Each peak traffic specification has a disk space limit.

|None|
|Disk capacity|To ensure the performance and storage space, each peak traffic specification has a disk space limit. That is, you need to purchase an allowed disk capacity. The peak traffic is also limited by the disk type. Message Queue for Apache Kafka supports the following disk types: -   Ultra disk:

    -   Standard Edition: supports a maximum of 120 MB/s peak traffic.
    -   Professional Edition: does not support the ultra disk
The maximum peak traffic is 120 MB/s.

-   \(Recommended\) SSD:
    -   Standard Edition: supports a maximum of 120 MB/s peak traffic.
    -   Professional Edition: supports a maximum of 2,000 MB/s peak traffic.

|The disk type cannot be changed after the order is placed. Select the disk type with caution. The 3-replica mechanism is used for both [cloud storage and local storage](/intl.en-US/Introduction/Storage engine comparison.md). For example, if your actual business storage capacity is 100 GB, you need to purchase 300 GB for three replicas.|
|Topic specifications|-   Topic specifications vary with peak traffic. For more information about the mappings between topic specifications and peak traffic values, see the billing rules for peak traffic in [Billing methods](#section_5z4_pg2_j7u).
-   In addition to the default number of partitions, 16 partitions are added for each additional topic. For example, you have purchased an instance that has 50 topics, 20 MB/s peak traffic, and 400 partitions by default. After you add 10 topics for this instance, the number of partitions of this instance is increased by 160, and the total number of partitions becomes 560.
-   The number of topics available for an instance of the Professional Edition is twice the number of purchased topics for this instance. For example, if you purchase a Professional Edition instance that has 50 topics, the actual number of topics you can create in the instance is 100.

|None|

**Note:**

-   You need to pay the fees of all billing items.
-   You can set the **Maximum Message Retention Period** parameter to conserve disk capacity. This parameter indicates the maximum retention period of a message when the disk capacity is sufficient. When the disk capacity is insufficient \(that is, the disk usage reaches 90%\), the old messages are deleted in advance to ensure service availability. By default, the maximum retention period of a message is 72 hours. This value ranges from 24 hours to 168 hours.
-   The number of API calls is not a billing item.

## Billing methods

Currently, Message Queue for Apache Kafka only supports subscription.

Formulas

Total fee = \(Unit price of peak traffic + Unit price of disk capacity × Purchased disk capacity/100 + Topic unit price × Number of added topics\) × Number of months

Billing rules

Billing rules include the rules for the peak traffic, disk capacity, and added topics:

-   Billing rules for peak traffic

    Billing rules for peak traffic are related to instance editions.

    |Peak traffic \(MB/s\)|Number of topics|Number of partitions|Unit price in Region 1 \(USD/month\)|Unit price in Region 2 \(USD/month\)|Unit price in Region 3 \(USD/month\)|
    |---------------------|----------------|--------------------|------------------------------------|------------------------------------|------------------------------------|
    |20|50|400|250|370|340|
    |30|50|500|360|530|480|
    |60|80|600|520|760|700|
    |90|100|800|660|960|880|
    |120|150|900|800|1,160|1,070|

    |Peak traffic \(MB/s\)|Number of topics|Number of partitions|Unit price in Region 1 \(USD/month\)|Unit price in Region 2 \(USD/month\)|Unit price in Region 3 \(USD/month\)|
    |---------------------|----------------|--------------------|------------------------------------|------------------------------------|------------------------------------|
    |20|50|1,100|600|870|800|
    |30|50|1,200|780|1,040|1,040|
    |60|80|1,400|1,130|1,510|1,510|
    |90|100|1,600|1,440|1,920|1,920|
    |120|150|1,800|1,750|2,330|2,330|
    |160|180|2,000|2,060|2,740|2,740|
    |200|200|2,200|2,980|3,970|3,970|
    |250|250|2,500|3,430|4,570|4,570|
    |300|300|3,000|3,880|5,170|5,170|
    |600|450|4,500|5,280|7,030|7,030|
    |800|500|5,000|6,220|8,280|8,280|
    |1,000|600|6,000|7,270|9,670|9,670|
    |1,200|700|7,000|8,100|10,780|10,780|
    |1,500|800|8,000|9,500|12,640|12,640|
    |1,800|900|9,000|10,910|14,520|14,520|
    |2,000|1,000|10,000|12,070|16,060|16,060|

-   Billing rules for disk capacity

    |Disk type|Disk size \(GB\)|Unit price in Region 1 \(USD/month\)|Unit price in Region 2 \(USD/month\)|Unit price in Region 3 \(USD/month\)|
    |---------|----------------|------------------------------------|------------------------------------|------------------------------------|
    |Ultra disk|100|6|8|8|
    |SSD|100|16|23|21|

-   Billing rules for added topics

    |Billing item|Number of topics|Unit price in Region 1 \(USD/month\)|Unit price in Region 2 \(USD/month\)|Unit price in Region 3 \(USD/month\)|
    |------------|----------------|------------------------------------|------------------------------------|------------------------------------|
    |Topic|1|7|11|10|


## Regions

Message Queue for Apache Kafka supports the following regions:

|Region|Region|
|------|------|
|Region 1|China \(Hangzhou\)|
|China \(Shanghai\)|
|China \(Qingdao\)|
|China \(Beijing\)|
|China \(Zhangjiakou-Beijing Winter Olympics\)|
|China \(Hohhot\)|
|China \(Shenzhen\)|
|China \(Heyuan\)|
|China \(Chengdu\)|
|Region 2|China \(Hong Kong\)|
|Singapore \(Singapore\)|
|Japan \(Tokyo\)|
|US \(Virginia\)|
|US \(Silicon Valley\)|
|Germany \(Frankfurt\)|
|UK \(London\)|
|Region 3|Malaysia \(Kuala Lumpur\)|
|India \(Mumbai\)|
|Indonesia \(Jakarta\)|

