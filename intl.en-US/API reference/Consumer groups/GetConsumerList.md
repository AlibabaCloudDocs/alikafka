# GetConsumerList

You can call this operation to query the consumer groups created on a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetConsumerList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetConsumerList|The operation that you want to perform. Set the value to GetConsumerList. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose consumer groups you want to query. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance whose consumer groups you want to query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|ConsumerList|Array| |The returned list of consumer groups. |
|ConsumerVO| | | |
|ConsumerId|String|CID\_c34a6f44915f80d70cb42c4b14ee40c3\_4|The name of the consumer group. |
|InstanceId|String|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where the consumer group is located. |
|RegionId|String|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where the consumer group is located. |
|Tags|Array| |The tags bound to the consumer group. |
|TagVO| | | |
|Key|String|test|The key of the tag bound to the consumer group. |
|Value|String|test|The value of the tag bound to the consumer group. |
|Message|String|operation success.|The returned message. |
|RequestId|String|808F042B-CB9A-4FBC-9009-00E7DDB6\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

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
<GetConsumerListResponse>
        <ConsumerList>
                <ConsumerVO>
                        <ConsumerId>CID_c34a6f44915f80d70cb42c4b14ee40c3_4</ConsumerId>
                        <InstanceId>alikafka_post-cn-v0h18sav****</InstanceId>
                        <RegionId>cn-hangzhou</RegionId>
                </ConsumerVO>
                <ConsumerVO>
                        <ConsumerId>CID_c34a6f44915f80d70cb42c4b14ee40c3_3</ConsumerId>
                        <InstanceId>alikafka_post-cn-v0h18sav****</InstanceId>
                        <RegionId>cn-hangzhou</RegionId>
                </ConsumerVO>
        </ConsumerList>
        <Message>operation success. </Message>
        <RequestId>808F042B-CB9A-4FBC-9009-00E7DDB6****</RequestId>
        <Success>true</Success>
        <Code>200</Code>
</GetConsumerListResponse>
```

`JSON` format

```
{
    "GetConsumerListResponse": {
        "ConsumerList": {
            "ConsumerVO": [
                {
                    "ConsumerId": "CID_c34a6f44915f80d70cb42c4b14ee40c3_4",
                    "InstanceId": "alikafka_post-cn-v0h18sav****",
                    "RegionId": "cn-hangzhou"
                },
                {
                    "ConsumerId": "CID_c34a6f44915f80d70cb42c4b14ee40c3_3",
                    "InstanceId": "alikafka_post-cn-v0h18sav****",
                    "RegionId": "cn-hangzhou"
                }
            ]
        },
        "Message": "operation success.",
        "RequestId": "808F042B-CB9A-4FBC-9009-00E7DDB6****",
        "Success": true,
        "Code": 200
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

