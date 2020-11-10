---
keyword: [kafka, rebalance, client, heartbeat]
---

# Why does rebalancing frequently occurs on the client?

The issue may occur if the major version of your Message Queue for Apache Kafka instance is outdated or the consumer does not have a separate thread to send heartbeat messages.

## Problem description

Rebalancing frequently occurs on the client.

## Cause

This issue may occur due to the following causes:

-   The major version of your Message Queue for Apache Kafka instance is version 0.10.
-   The consumer of Message Queue for Apache Kafka does not have a separate thread to send heartbeat messages. Instead, the consumer couples the sending of heartbeat messages with the poll method. Therefore, if the consumption response is slow, the request to send heartbeat messages times out, and thus rebalancing occurs.
    -   `session.timeout.ms`: specifies the timeout period of requests to send heartbeat messages. You can specify a custom value based on your business requirements.
    -   `max.poll.records`: specifies the maximum number of messages returned in a single call to the poll method.

## Solution

1.  Check the major version of your Message Queue for Apache Kafka instance.

    If the major version of your instance is version 0.10, we recommend that you upgrade the major version to version 0.10.2, which is more stable. For more information, see [Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md).

2.  Adjust the configuration of the following parameters:

    -   Set the `session.timeout.ms` parameter to a value smaller than or equal to 30000. We recommend that you set the value to 25000.

    -   Set the `max.poll.records` parameter to a value far smaller than the limit value. The limit value is calculated by using the following formula: Number of messages consumed by a single thread per second × Number of consumption threads × Number of seconds specified by the `session.timeout` parameter.

3.  Improve the consumption speed of the consumer.


