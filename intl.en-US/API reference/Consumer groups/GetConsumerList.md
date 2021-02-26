# GetConsumerList

Queries consumer groups in a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetConsumerList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetConsumerList|The operation that you want to perform. Set the value to

 **GetConsumerList**. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request is successful. |
|ConsumerList|Array of ConsumerVO| |The list of consumer groups. |
|ConsumerVO| | | |
|ConsumerId|String|CID\_c34a6f44915f80d70cb42c4b14\*\*\*|The name of the consumer group. |
|InstanceId|String|alikafka\_post-cn-v0h18sav\*\*\*\*|The ID of the instance. |
|RegionId|String|cn-hangzhou|The region ID of the instance. |
|Remark|String|test|The description of the consumer group. |
|Tags|Array of TagVO| |The tags bound to the consumer group. |
|TagVO| | | |
|Key|String|test|The key of the resource tag. |
|Value|String|test|The value of the resource tag. |
|Message|String|operation success.|The response message. |
|RequestId|String|808F042B-CB9A-4FBC-9009-00E7DDB6\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetConsumerList
&RegionId=cn-hangzhou
&InstanceId=alikafka_post-cn-v0h18sav****
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
                  <Tags>
            </Tags>
                  <Remark>test</Remark>
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
	            "Tags": {
	            	"TagVO": []
	            },
                "Remark": "test"
            }
        ]
    },
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

