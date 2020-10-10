# Introduction

You can call the resource reports API operations of CloudMonitor to query the usage of resources, including an instance, a consumer group, and a topic of Message Queue for Apache Kafka.

## Metrics

The resource reports API of CloudMonitor supports the following metrics for Message Queue for Apache Kafka.

|Metric|Description|Unit|Dimension|
|------|-----------|----|---------|
|instance\_disk\_capacity|The disk usage of a Message Queue for Apache Kafka instance.|%|instanceId|
|instance\_message\_input|The production traffic of a Message Queue for Apache Kafka instance.|bytes/s|instanceId|
|instance\_message\_output|The consumption traffic of a Message Queue for Apache Kafka instance.|bytes/s|instanceId|
|message\_accumulation|The number of unconsumed messages for a consumer group.|Count|-   instanceId
-   consumerGroup |
|message\_accumulation\_onetopic|The number of unconsumed messages in the topic for a consumer group.|Count|-   instanceId
-   consumerGroup
-   topic |
|topic\_message\_input|The production traffic of a topic.|bytes/s|-   instanceId
-   topic |
|topic\_message\_output|The consumption traffic of a topic.|bytes/s|-   instanceId
-   topic |

## List of operations by function

-   [DescribeMetricMetaList](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricMetaList.md)
-   [DescribeMetricLast](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricLast.md)
-   [DescribeMetricList](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricList.md)
-   [DescribeMetricData](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricData.md)
-   [DescribeMetricTop](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricTop.md)
-   [DescribeProjectMeta](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeProjectMeta.md)

## API operations

For the information about how to call the CloudMonitor resource reports API operations, see [Request method](/intl.en-US/API Reference/Request method.md).

