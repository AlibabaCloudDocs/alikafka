# TagResources

Attaches a tag to a resource.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=TagResources&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|TagResources|The operation that you want to perform. Set the value to

 **TagResources**. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance resides. |
|ResourceId.N|RepeatList|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the resource to which you want to attach a tag. Take note of the following rules when you specify a resource ID:

 -   The resource ID of an instance is the value of the instanceId parameter.
-   The resource ID of a topic is the value of the Kafka\_alikafka\_instanceId\_topic parameter.
-   The resource ID of a consumer group is the value of the Kafka\_alikafka\_instanceId\_consumerGroup parameter.

 For example, the resources to which the tag is to be attached include the alikafka\_post-cn-v0h1fgs2xxxx instance, the test-topic topic, and the test-consumer-group consumer group. In this case, their resource IDs are alikafka\_post-cn-v0h1fgs2xxxx, Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic, and Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group. |
|ResourceType|String|Yes|instance|The type of the resource. Valid values:

 -   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|Tag.N.Key|String|Yes|FinanceDept|The key of the tag.

 -   Valid values of N: 1 to 20.
-   You must set this parameter.
-   The tag key can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|Tag.N.Value|String|No|FinanceJoshua|The value of the tag.

 -   Valid values of N: 1 to 20.
-   This parameter is optional.
-   The tag value can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=TagResources
&RegionId=cn-hangzhou
&ResourceId.1=alikafka_post-cn-v0h1fgs2****
&ResourceType=INSTANCE
&Tag.1.Key=FinanceDept
&<Common request parameters>
```

Sample success responses

`XML` format

```
<TagResourcesResponse>
      <RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
</TagResourcesResponse>
```

`JSON` format

```
{
    "RequestId": "C46FF5A8-C5F0-4024-8262-B16B639225A0"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

