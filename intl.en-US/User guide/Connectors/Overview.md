---
keyword: [kafka, connector]
---

# Overview

Message Queue for Apache Kafka provides fully managed and maintenance-free connectors to synchronize data between Message Queue for Apache Kafka and other Alibaba Cloud services. This topic describes the types, procedure, and limits of connectors. It also describes how to synchronize data between services in different regions by using connectors.

**Note:** The connector feature of Message Queue for Apache Kafka is in public preview. This feature is independent of Message Queue for Apache Kafka instances. Therefore, you are not charged on the Message Queue for Apache Kafka side when you use a connector to synchronize data between Message Queue for Apache Kafka and another Alibaba Cloud service. Alibaba Cloud does not provide service level agreement \(SLA\) commitments for the connector feature in public preview. For information about the SLA commitments and billing of the services that are related to the connector feature, see the documentation of the services.

## Types of connectors

Message Queue for Apache Kafka provides two categories of connectors:

-   Sink connector: Sink connectors are used to synchronize data from Message Queue for Apache Kafka to other Alibaba Cloud services.

    |Connector|Description|References|
    |---------|-----------|----------|
    |Function Compute sink connector|Synchronizes data from Message Queue for Apache Kafka to Function Compute.|[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)|
    |MaxCompute sink connector|Synchronizes data from Message Queue for Apache Kafka to MaxCompute.|[Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)|
    |OSS sink connector|Synchronizes data from Message Queue for Apache Kafka to Object Storage Service \(OSS\).|[Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an OSS sink connector.md)|
    |Elasticsearch sink onnector|Synchronizes data from Message Queue for Apache Kafka to Elasticsearch.|[Create an Elasticsearch sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an Elasticsearch sink connector.md)|

-   Source connector: Source connectors are used to synchronize data from other Alibaba Cloud services to Message Queue for Apache Kafka.

    |Connector|Description|References|
    |---------|-----------|----------|
    |MySQL source connector|Synchronizes data from ApsaraDB RDS for MySQL to Message Queue for Apache Kafka.|[Create a MySQL source connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MySQL source connector.md)|


## Procedure

Perform the following steps to use connectors:

1.  [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md)
2.  Create a connector.
    -   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
    -   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)
    -   [Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an OSS sink connector.md)
    -   [Create an Elasticsearch sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an Elasticsearch sink connector.md)
    -   [Create a MySQL source connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MySQL source connector.md)
3.  Perform the following operations as required:
    -   [View task configurations of a connector](/intl.en-US/User guide/Connectors/View task configurations of a connector.md)
    -   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md)
    -   [Suspend a connector](/intl.en-US/User guide/Connectors/Suspend a connector.md)
    -   [Resume a connector](/intl.en-US/User guide/Connectors/Resume a connector.md)
    -   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md)
    -   [Modify connector configurations](/intl.en-US/User guide/Connectors/Modify connector configurations.md)
    -   [Test a connector](/intl.en-US/User guide/Connectors/Test a connector.md)
    -   [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md)

## Limits

The following table lists the limits of Message Queue for Apache Kafka on connectors.

|Item|Limit|
|----|-----|
|Maximum number of connectors|You can create up to three connectors for each instance.|
|Regions|-   China \(Hangzhou\)
-   China \(Shanghai\)
-   China \(Beijing\)
-   China \(Zhangjiakou\)
-   China \(Hohhot\)
-   China \(Shenzhen\)
-   China \(Chengdu\)
-   China \(Hong Kong\)
-   Singapore \(Singapore\)
-   Japan \(Tokyo\) |

**Note:** To increase the number connectors for your instance or use connectors in other regions, submit a[ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352) to Message Queue for Apache Kafka Customer Services.

## Cross-region data synchronization

You must enable Internet access for a connector to synchronize data from an Alibaba Cloud service in one region to an Alibaba Cloud service in another region over the Internet. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

**Note:** If you need to use MySQL source connectors to synchronize data across regions, you must first activate Cloud Enterprise Network \(CEN\). For more information, see [Create a MySQL source connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MySQL source connector.md).

