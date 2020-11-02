# TagResources

You can call this operation to bind a tag to a resource.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=TagResources&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|TagResources|The operation that you want to perform. Set the value to TagResources. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the resource. |
|ResourceId.N|RepeatList|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of resource N to which the tag will be bound. The resource ID follows these rules:

 -   If the resource is an instance, the resource ID uses the same syntax as the value of the instanceId parameter.
-   If the resource is a topic, the resource ID uses the same syntax as the value of the Kafka\_instanceId\_topic parameter.
-   If the resource is a consumer group, the resource ID uses the same syntax as the value of the Kafka\_instanceId\_consumerGroup parameter.

 For example, the resources to which the tag will be bound include the alikafka\_post-cn-v0h1fgs2xxxx instance, the test-topic topic, and the test-consumer-group consumer group. In this case, their resource IDs are alikafka\_post-cn-v0h1fgs2xxxx, Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic, and Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group respectively. |
|ResourceType|String|Yes|instance|The type of the resource. The value is an enumerated value. Valid values:

 -   **Instance**
-   **Topic**
-   **Consumer Group** |
|Tag.N.Key|String|Yes|FinanceDept|The key of tag N to be bound to the resource. Valid values of N:

 -   1 to 20
-   This parameter cannot be an empty string.
-   It can be up to 128 characters in length. It cannot start with aliyun or acs: and cannot contain http:// or https://. |
|Tag.N.Value|String|No|FinanceJoshua|The value of tag N to be bound to the resource.

 -   Valid values of N: 1 to 20
-   This parameter can be an empty string.
-   It can be up to 128 characters in length. It cannot start with aliyun or acs: and cannot contain http:// or https://. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=TagResources
&RegionId=cn-hangzhou
&ResourceId.1=alikafka_post-cn-v0h1fgs2****
&ResourceType=instance
&Tag.1.Key=FinanceDept
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
```

`JSON` format

```
{
    "RequestId":"C46FF5A8-C5F0-4024-8262-B16B639225A0"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

