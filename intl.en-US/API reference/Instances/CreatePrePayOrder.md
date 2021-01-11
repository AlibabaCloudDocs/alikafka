# CreatePrePayOrder

Creates a subscription Message Queue for Apache Kafka instance.

Before you call this operation, make sure that you have understood the billing methods and pricing of subscription Message Queue for Apache Kafka instances. For more information, see[Billing](~84737~).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreatePrePayOrder&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreatePrePayOrder|The operation that you want to perform. Set the value to **CreatePrePayOrder**. |
|DeployType|Integer|Yes|5|The deployment type. Set the value to 5, which indicates that the subscription Message Queue for Apache Kafka instance is deployed in a virtual private cloud \(VPC\).

  |
|DiskSize|Integer|Yes|900|The size of the disk to be configured for the subscription Message Queue for Apache Kafka instance.

 For more information about the value range, see[Billing](~~84737~~). |
|DiskType|String|Yes|1|The type of the disk to be configured for the subscription Message Queue for Apache Kafka instance. Valid values:

 -   **0:** Ultra disk
-   **1:** SSD |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the subscription Message Queue for Apache Kafka instance is located. |
|TopicQuota|Integer|Yes|50|The number of topics to be configured for the subscription Message Queue for Apache Kafka instance.

 -   The default value varies with the traffic specifications. If the value exceeds the default value, additional fees are charged.
-   For more information about the value range, see[Billing](~~84737~~). |
|IoMax|Integer|No|20|The peak traffic to be configured for the subscription Message Queue for Apache Kafka instance \(not recommended\).

 -   You must specify either the peak traffic or traffic specifications. If you specify both fields, the traffic specifications prevail. We recommend that you specify only the traffic specifications.
-   For more information about the value range, see[Billing](~~84737~~). |
|EipMax|Integer|No|40|The public traffic to be configured for the subscription Message Queue for Apache Kafka instance.

 Internet access is not allowed. Therefore, you do not need to set this value.

  |
|SpecType|String|No|normal|The edition of the subscription Message Queue for Apache Kafka instance. Valid values:

 -   **normal:**Standard Edition \(High Write\)
-   **professional:**Professional Edition \(High Write\)
-   **professionalForHighRead:**Professional Edition \(High Read\)

 For more information about these instance editions, see[Billing](~~84737~~). |
|IoMaxSpec|String|No|alikafka.hw.2xlarge|The traffic specifications \(recommended\).

 -   You must specify either the peak traffic or traffic specifications. If you specify both fields, the traffic specifications prevail. We recommend that you specify only the traffic specifications.
-   For more information about the value range, see[Billing](~~84737~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If 200 is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|OrderId|String|20497346575\*\*\*\*|The order ID of the subscription Message Queue for Apache Kafka instance. |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the call is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreatePrePayOrder
&DeployType=5
&DiskSize=900
&DiskType=1
&RegionId=cn-hangzhou
&TopicQuota=50
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreatePrePayOrderResponse>
      <Message>operation success. </Message>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <OrderId>20497346575****</OrderId>
      <Code>200</Code>
      <Success>true</Success>
</CreatePrePayOrderResponse>
```

`JSON` format

```
{
    "Message":"operation success.",
    "RequestId":"06084011-E093-46F3-A51F-4B19A8AD****",
    "OrderId":"20497346575****",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

