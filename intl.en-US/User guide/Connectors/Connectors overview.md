---
keyword: [kafka, connector]
---

# Connectors overview

Message Queue for Apache Kafka provides fully managed and maintenance-free connectors for data synchronization between Message Queue for Apache Kafka and other Alibaba Cloud services. The connector feature is in public preview. This topic describes the types, procedure, limits, and cross-region access of connectors.

## Types of connectors

Message Queue for Apache Kafka only provides sink connectors to export data from Message Queue for Apache Kafka to other Alibaba Cloud services. Message Queue for Apache Kafka supports the following types of sink connectors:

|Connector type|Task type|Description|Documentation|
|--------------|---------|-----------|-------------|
|Function Compute sink connector|KAFKA2FC|Exports data from Message Queue for Apache Kafka to Function Compute.|[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)|
|MaxCompute sink c.onnector|KAFKA2ODPS|Exports data from Message Queue for Apache Kafka to MaxCompute.|[Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)|

## Procedure

Perform the following steps to use connectors:

1.  [Enable the Connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).
2.  Create a connector.
    -   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md).
    -   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md).
3.  Manage connectors.
    -   [View the task configurations of a connector](/intl.en-US/User guide/Connectors/View the task configurations of a connector.md).
    -   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md).
    -   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md).
    -   [Modify the connector description](/intl.en-US/User guide/Connectors/Modify connector descriptions.md).

## Limits

During the public preview, you can create up to three connector tasks for a single instance. The number of connector tasks has no lower limit.

## Cross-region access

If you need to access other Alibaba Cloud services in other regions by using a connector, you must enable public network access for the connector. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

