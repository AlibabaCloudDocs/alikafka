# UntagResources

Detaches a tag from a resource.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=UntagResources&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UntagResources|The operation that you want to perform. Set the value to

**UntagResources**. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance resides. |
|ResourceId.N|RepeatList|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the resource from which you want to detach a tag. Take note of the following rules when you specify a resource ID:

-   The resource ID of an instance is the value of the instanceId parameter.
-   The resource ID of a topic is the value of the Kafka\_instanceId\_topic parameter.
-   The resource ID of a consumer group is the value of the Kafka\_instanceId\_consumerGroup parameter.

For example, the resources from which the tag is to be detached include the alikafka\_post-cn-v0h1fgs2xxxx instance, the test-topic topic, and the test-consumer-group consumer group. In this case, their resource IDs are alikafka\_post-cn-v0h1fgs2xxxx, Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic, and Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group. |
|ResourceType|String|Yes|instance|The type of the resource. Valid values:

-   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|TagKey.N|RepeatList|No|FinanceDept|The key of the tag.

-   Valid values of N: 1 to 20.
-   If you do not set this parameter and set the All parameter to true, all tag keys are matched.
-   The tag key can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|All|Boolean|No|false|Specifies whether to detach all the tags that are attached to the resource. This parameter takes effect only when the TagKey.N parameter is not set. Default value: **false**. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=alikafka_post-cn-v0h1fgs2****
&ResourceType=INSTANCE
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UntagResourcesResponse>
      <RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
</UntagResourcesResponse>
```

`JSON` format

```
{
    "RequestId": "C46FF5A8-C5F0-4024-8262-B16B639225A0"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

