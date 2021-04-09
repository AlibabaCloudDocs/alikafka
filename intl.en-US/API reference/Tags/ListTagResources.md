# ListTagResources

Queries the tags that are attached to a resource.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=ListTagResources&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListTagResources|The operation that you want to perform. Set the value to

 **ListTagResources**. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance resides. |
|ResourceType|String|Yes|instance|The type of the resource whose tags you want to query. Valid values:

 -   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|ResourceId.N|RepeatList|No|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the resource. Take note of the following rules when you specify a resource ID:

 -   The resource ID of an instance is the value of the instanceId parameter.
-   The resource ID of a topic is the value of the Kafka\_alikafka\_instanceId\_topic parameter.
-   The resource ID of a consumer group is the value of the Kafka\_alikafka\_instanceId\_consumerGroup parameter.

 For example, the resources whose tags you want to query include the alikafka\_post-cn-v0h1fgs2xxxx instance, the test-topic topic, and the test-consumer-group consumer group. In this case, their resource IDs are alikafka\_post-cn-v0h1fgs2xxxx, Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic, and Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group. |
|Tag.N.Key|String|No|FinanceDept|The key of the tag.

 -   Valid values of N: 1 to 20.
-   If this parameter is not set, the keys of all tags are matched.
-   The tag key can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|Tag.N.Value|String|No|FinanceJoshua|The value of the tag.

 -   Valid values of N: 1 to 20.
-   If you do not specify a tag key, you cannot specify a tag value. If this parameter is not set, the values of all tags are matched.
-   The tag value can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|NextToken|String|No|caeba0bbb2be03f84eb48b699f0a4883|The token that is used to start the next query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|NextToken|String|caeba0bbb2be03f84eb48b699f0a4883|The token that is returned for the next query. |
|RequestId|String|DE65F6B7-7566-4802-9007-96F2494AC5XX|The ID of the request. |
|TagResources|Array of TagResource| |The returned collection of resources and tags, including the resource ID, resource type, and key-value pairs of the tags. |
|TagResource| | | |
|ResourceId|String|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the resource. The resource ID follows these rules:

 -   The resource ID of an instance is the value of the instanceId parameter.
-   The resource ID of a topic is the value of the Kafka\_alikafka\_instanceId\_topic parameter.
-   The resource ID of a consumer group is the value of the Kafka\_alikafka\_instanceId\_consumerGroup parameter.

 For example, the resources whose tags you query include the alikafka\_post-cn-v0h1fgs2xxxx instance, the test-topic topic, and the test-consumer-group consumer group. In this case, their resource IDs are alikafka\_post-cn-v0h1fgs2xxxx, Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic, and Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group. |
|ResourceType|String|instance|The type of the resource. Valid values:

 -   **Instance**
-   **Topic**
-   **Consumergroup** |
|TagKey|String|FinanceDept|The key of the tag. |
|TagValue|String|FinanceJoshua|The value of the tag. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ListTagResources
&RegionId=cn-hangzhou
&ResourceType=INSTANCE
&ResourceId=alikafka_post-cn-v0h1fgs2****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ListTagResourcesResponse>
      <RequestId>DE65F6B7-7566-4802-9007-96F2494AC5XX</RequestId>
      <NextToken>caeba0bbb2be03f84eb48b699f0a4883</NextToken>
      <TagResources>
            <TagResource>
                  <ResourceId>alikafka_post-cn-v0h1fgs2****</ResourceId>
                  <TagKey>FinanceDept</TagKey>
                  <ResourceType>ALIYUN::ALIKAFKA::INSTANCE</ResourceType>
                  <TagValue>FinanceJoshua</TagValue>
            </TagResource>
      </TagResources>
</ListTagResourcesResponse>
```

`JSON` format

```
{
    "RequestId": "DE65F6B7-7566-4802-9007-96F2494AC5XX",
    "NextToken": "caeba0bbb2be03f84eb48b699f0a4883",
    "TagResources": {
        "TagResource": [
          {
            "ResourceId": "alikafka_post-cn-v0h1fgs2****",
            "TagKey": "FinanceDept",
            "ResourceType": "ALIYUN::ALIKAFKA::INSTANCE",
            "TagValue": "FinanceJoshua"
          }  
        ]
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

