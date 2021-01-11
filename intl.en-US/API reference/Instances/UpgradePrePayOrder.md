# UpgradePrePayOrder

Upgrades a subscription Message Queue for Apache Kafka instance.

Before you call this operation, make sure that you have understood the billing methods and pricing of subscription Message Queue for Apache Kafka instances. For more information, see[Billing](~84737~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UpgradePrePayOrder&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpgradePrePayOrder|The operation that you want to perform. Set the value to **UpgradePrePayOrder**. |
|DiskSize|Integer|Yes|900|The size of the disk to be configured for the subscription Message Queue for Apache Kafka instance.

 -   The specified disk size must be greater than or equal to the current disk size of the subscription Message Queue for Apache Kafka instance.
-   For more information about the value range, see[Billing](~~84737~~). |
|InstanceId|String|Yes|alikafka\_post-cn-mp919o4v\*\*\*\*|The ID of the subscription Message Queue for Apache Kafka instance to be upgraded. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the subscription Message Queue for Apache Kafka instance is located. |
|TopicQuota|Integer|Yes|50|The number of topics to be configured for the subscription Message Queue for Apache Kafka instance.

 -   The specified number of topics must be greater than or equal to the current number of topics for the subscription Message Queue for Apache Kafka instance.
-   The default value varies with the traffic specifications. If the value exceeds the default value, additional fees are charged.
-   For more information about the value range, see[Billing](~~84737~~). |
|IoMax|Integer|No|40|The peak traffic to be configured for the subscription Message Queue for Apache Kafka instance \(not recommended\).

 -   The specified peak traffic must be greater than or equal to the current peak traffic configured for the subscription Message Queue for Apache Kafka instance.
-   You must specify either the peak traffic or traffic specifications. If you specify both fields, the traffic specifications prevail. We recommend that you specify only the traffic specifications.
-   For more information about the value range, see[Billing](~~84737~~). |
|SpecType|String|No|professional|The edition of the subscription Message Queue for Apache Kafka instance. Valid values:

 -   **normal:**Standard Edition \(High Write\)
-   **professional:**Professional Edition \(High Write\)
-   **professionalForHighRead:**Professional Edition \(High Read\)

 You cannot downgrade a subscription Message Queue for Apache Kafka instance from Professional Edition to Standard Edition. For more information about these instance editions, see[Billing](~~84737~~). |
|EipMax|Integer|No|0|The public traffic to be configured for the subscription Message Queue for Apache Kafka instance.

 Internet access is not allowed. Therefore, you do not need to set this value.

  |
|IoMaxSpec|String|No|alikafka.hw.2xlarge|The traffic specifications \(recommended\).

 -   The specified traffic specifications must be greater than or equal to the current traffic specifications.
-   You must specify either the peak traffic or traffic specifications. If you specify both fields, the traffic specifications prevail. We recommend that you specify only the traffic specifications.
-   For more information about the value range, see[Billing](~~84737~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If 200 is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the call is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=UpgradePrePayOrder
&DiskSize=900
&InstanceId=alikafka_post-cn-mp919o4v****
&RegionId=cn-hangzhou
&TopicQuota=50
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpgradePrePayOrderResponse>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015***</RequestId>
      <Message>operation success. </Message>    
      <Success>true</Success>
      <Code>200</Code>
</UpgradePrePayOrderResponse>
```

`JSON` format

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015***",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

