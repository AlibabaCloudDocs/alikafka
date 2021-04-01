---
keyword: [Kafka, connector]
---

# Overview

Message Queue for Apache Kafka provides fully-managed and maintenance-free connectors for you to synchronize data between Message Queue for Apache Kafka and other Alibaba Cloud services. This topic describes the types, procedure, and usage limits of connectors. It also describes how to use connectors to synchronize data across regions.

**Note:** The connector feature of Message Queue for Apache Kafka is in public preview. This feature is independent of Message Queue for Apache Kafka instances. Therefore, Message Queue for Apache Kafka does not charge you when you use a connector to synchronize data between Message Queue for Apache Kafka and another Alibaba Cloud service. Alibaba Cloud does not provide service level agreement \(SLA\) commitments for the connector feature in public preview. For information about the SLA commitments and billing of the services that are related to the connector feature, see the documentation of the services.

## Types of connectors

Message Queue for Apache Kafka provides two categories of connectors:

-   Sink connector: Sink connectors are used to synchronize data from Message Queue for Apache Kafka to other Alibaba Cloud services.

    |Connector|Task type|Description|Documentation|
    |---------|---------|-----------|-------------|
    |Function Compute sink connector|KAFKA2FC|Exports data from Message Queue for Apache Kafka to Function Compute.|[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)|
    |MaxCompute sink connector|KAFKA2ODPS|Exports data from Message Queue for Apache Kafka to MaxCompute.|[Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)|
    |OSS sink connector|KAFKA2OSS|Exports data from Message Queue for Apache Kafka to Object Storage Service \(OSS\).|[Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a connector to synchronize data to OSS.md)|

-   Source connector: Source connectors are used to synchronize data from other Alibaba Cloud services to Message Queue for Apache Kafka.

    |Connector|Task type|Description|Documentation|
    |---------|---------|-----------|-------------|
    |MySQL sink connector|MySQL2KAFKA|Imports data from ApsaraDB RDS for MySQL to Message Queue for Apache Kafka.|[Create a MySQL source connector]()|


## Procedure

To use connectors, perform the following steps:

1.  [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
2.  Create a connector.
    -   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md).
    -   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md).
    -   [Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a connector to synchronize data to OSS.md).
    -   [Create a MySQL source connector]().
3.  Perform the following operations as required:
    -   [View task configurations of a connector](/intl.en-US/User guide/Connectors/View task configurations of a connector.md).
    -   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
    -   [Suspend a connector](/intl.en-US/User guide/Connectors/Suspend a connector.md).
    -   [Resume a connector](/intl.en-US/User guide/Connectors/Resume a connector.md).
    -   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md).
    -   [Modify the description of a connector](/intl.en-US/User guide/Connectors/Modify the description of a connector.md).

## Limits

The following table describes the usage limits of connectors in Message Queue for Apache Kafka.

|Item|Description|
|----|-----------|
|Maximum number of connectors|You can create up to three connectors for each instance.|
|Regions|-   China \(Hangzhou\)
-   China \(Shanghai\)
-   China \(Beijing\)
-   China \(Zhangjiakou\)
-   China \(Hohhot\)
-   China \(Shenzhen\)
-   China \(Chengdu\)
-   China \(Hong Kong\)
-   Singapore
-   Japan \(Tokyo\) |

**Note:** If you need to create more than three connectors for each instance or use connectors in other regions, submit a [ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352) to request technical support for Message Queue for Apache Kafka.

## Cross-region data synchronization

If you need to use a connector to synchronize data to an Alibaba Cloud service in another region over the Internet, you must enable Internet access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

**Note:** If you need to use MySQL source connectors to synchronize data across regions, you must first activate Cloud Enterprise Network \(CEN\). For more information, see [Create a MySQL source connector]().

