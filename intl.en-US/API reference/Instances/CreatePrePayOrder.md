# CreatePrePayOrder

Creates a subscription Message Queue for Apache Kafka instance.

Before you call this operation, make sure that you have understood the billing methods and pricing of subscription Message Queue for Apache Kafka instances. For more information, see[Billing](~~84737~~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreatePrePayOrder&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreatePrePayOrder|The operation that you want to perform. Set the value to

**CreatePrePayOrder**. |
|DeployType|Integer|Yes|5|The deployment mode of the instance. Valid values:

-   **4**: Internet and virtual private cloud \(VPC\) type
-   **5**: VPC type |
|DiskSize|Integer|Yes|900|The size of the disk for the instance.

For more information about the valid values, see [Billing](~~84737~~). |
|DiskType|String|Yes|1|The disk type for the instance. Valid values:

-   **0**: ultra disk
-   **1**: solid state drive \(SSD\) |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|TopicQuota|Integer|Yes|50|The number of topics for the instance.

-   The default value varies with the traffic specifications. If the value exceeds the default value, additional fees are charged.
-   For more information about the valid values, see [Billing](~~84737~~). |
|IoMax|Integer|No|20|The maximum traffic for the instance \(not recommended\).

-   You must specify the maximum traffic or the traffic specification. If you specify both fields, the traffic specification prevails. We recommend that you specify only the traffic specification.
-   For more information about the valid values, see [Billing](~~84737~~). |
|EipMax|Integer|No|40|The public traffic for the instance.

-   This parameter is required if DeployType is set to **4**.
-   For more information about the valid values, see[Pay-as-you-go](~~72142~~). |
|SpecType|String|No|normal|The edition of the instance. Valid values:

-   **normal:** Standard Edition \(High Write\)
-   **professional:** Professional Edition \(High Write\)
-   **professionalForHighRead:** Professional Edition \(High Read\)

For more information about these instance editions, see[Billing](~~84737~~). |
|IoMaxSpec|String|No|alikafka.hw.2xlarge|The traffic specification \(recommended\).

-   You must specify the maximum traffic or the traffic specification. If you specify both fields, the traffic specification prevails. We recommend that you specify only the traffic specification.
-   For more information about the valid values, see [Billing](~~84737~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|OrderId|String|20497346575\*\*\*\*|The ID of the order. |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreatePrePayOrder
&RegionId=cn-hangzhou
&TopicQuota=50
&DiskType=1
&DiskSize=900
&DeployType=5
&IoMax=20
&SpecType=normal
&IoMaxSpec=alikafka.hw.2xlarge
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreatePrePayOrderResponse>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <Message>operation success. </Message>
      <OrderId>20497346575****</OrderId>
      <Code>200</Code>
      <Success>true</Success>
</CreatePrePayOrderResponse>
```

`JSON` format

```
{
    "RequestId":"06084011-E093-46F3-A51F-4B19A8AD****",
    "Message":"operation success.",
    "OrderId":"20497346575****",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

