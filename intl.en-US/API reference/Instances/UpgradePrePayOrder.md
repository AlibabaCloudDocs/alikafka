# UpgradePrePayOrder

Upgrades a subscription Message Queue for Apache Kafka instance.

Before you call this operation, make sure that you have understood the billing methods and pricing of subscription Message Queue for Apache Kafka instances. For more information, see [Billing](~~84737~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UpgradePrePayOrder&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpgradePrePayOrder|The operation that you want to perform. Set the value to

**UpgradePrePayOrder**. |
|DiskSize|Integer|Yes|900|The size of the disk for the instance.

-   The specified disk size must be at least the current disk size of the instance.
-   For more information about the valid values, see [Billing](~~84737~~). |
|InstanceId|String|Yes|alikafka\_post-cn-mp919o4v\*\*\*\*|The ID of the instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|TopicQuota|Integer|Yes|50|The number of topics for the instance.

-   The specified number of topics must be at least the current number of topics of the instance.
-   The default value varies with the traffic specifications. If the value exceeds the default value, additional fees are charged.
-   For more information about the valid values, see [Billing](~~84737~~). |
|IoMax|Integer|No|40|The maximum traffic for the instance \(not recommended\).

-   The specified maximum traffic must be at least the current maximum traffic of the instance.
-   You must specify the maximum traffic or the traffic specification. If you specify both fields, the traffic specification prevails. We recommend that you specify only the traffic specification.
-   For more information about the valid values, see [Billing](~~84737~~). |
|SpecType|String|No|professional|The edition of the instance. Valid values:

-   **normal:**Standard Edition \(High Write\)
-   **professional:**Professional Edition \(High Write\)
-   **professionalForHighRead:** Professional Edition \(High Read\)

You cannot downgrade an instance from the Professional Edition to the Standard Edition. For more information about these instance editions, see [Billing](~~84737~~). |
|EipMax|Integer|No|0|The public traffic for the instance.

-   The specified public traffic must be at least the current public traffic of the instance.
-   This parameter is required for instances of the Internet and VPC type.
-   For more information about the valid values, see [Billing](~~84737~~). |
|IoMaxSpec|String|No|alikafka.hw.2xlarge|The traffic specification \(recommended\).

-   The specified traffic specification must be at least the current traffic specification of the instance.
-   You must specify the maximum traffic or the traffic specification. If you specify both fields, the traffic specification prevails. We recommend that you specify only the traffic specification.
-   For more information about the valid values, see [Billing](~~84737~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=UpgradePrePayOrder
&RegionId=cn-hangzhou
&TopicQuota=50
&DiskSize=900
&InstanceId=alikafka_post-cn-mp919o4v****
&IoMax=40
&SpecType=professional
&EipMax=200
&IoMaxSpec=alikafka.hw.2xlarge
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

