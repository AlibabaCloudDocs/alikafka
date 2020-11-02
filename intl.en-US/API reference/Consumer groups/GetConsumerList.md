# GetConsumerList

Queries consumer groups.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetConsumerList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetConsumerList|The operation that you want to perform. Set the value to **GetConsumerList**. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose consumer groups you want to query. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|ConsumerList|Array of ConsumerVO| |The list of consumer groups. |
|ConsumerVO| | | |
|ConsumerId|String|CID\_c34a6f44915f80d70cb42c4b14\*\*\*|The name of the consumer group. |
|InstanceId|String|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the instance that was queried. |
|RegionId|String|cn-hangzhou|The ID of the region where the instance is located. |
|Remark|String|test|The description of the consumer group. |
|Tags|Array of TagVO| |The tags bound to the consumer group. |
|TagVO| | | |
|Key|String|test|The key of the resource tag. |
|Value|String|test|The value of the resource tag. |
|Message|String|operation success.|The response message. |
|RequestId|String|808F042B-CB9A-4FBC-9009-00E7DDB6\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetConsumerList
&InstanceId=alikafka_post-cn-v0h18sav****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetConsumerList>
      <RequestId>808F042B-CB9A-4FBC-9009-00E7DDB6****</RequestId>
      <Message>operation success. </Message>
      <Code>200</Code>
      <ConsumerList>
            <ConsumerVO>
                  <InstanceId>alikafka_post-cn-v0h18sav****</InstanceId>
                  <ConsumerId>CID_c34a6f44915f80d70cb42c4b14***</ConsumerId>
                  <RegionId>cn-hangzhou</RegionId>
                  <Remark>test</Remark>
            </ConsumerVO>
            <ConsumerVO>
                  <Tags>
                        <TagVO>
                              <Value>test</Value>
                              <Key>test</Key>
                        </TagVO>
                  </Tags>
            </ConsumerVO>
      </ConsumerList>
      <Success>true</Success>
</GetConsumerList>
```

`JSON` format

```
{
    "RequestId": "808F042B-CB9A-4FBC-9009-00E7DDB6****",
    "Message": "operation success.",
    "Code": 200,
    "ConsumerList": {
        "ConsumerVO": [
            {
                "InstanceId": "alikafka_post-cn-v0h18sav****",
                "ConsumerId": "CID_c34a6f44915f80d70cb42c4b14***",
                "RegionId": "cn-hangzhou",
                "Remark": "test"
            },
            {
                "Tags": {
                    "TagVO": {
                        "Value": "test",
                        "Key": "test"
                    }
                }
            }
        ]
    },
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

