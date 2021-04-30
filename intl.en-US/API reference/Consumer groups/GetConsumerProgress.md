# GetConsumerProgress

Queries the status of a consumer group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetConsumerProgress&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetConsumerProgress|The operation that you want to perform. Set the value to

 **GetConsumerProgress**. |
|ConsumerId|String|Yes|kafka-test|The name of the consumer group that you want to query. |
|InstanceId|String|Yes|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the consumer group. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance resides. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. If 200 is returned, the request is successful. |
|ConsumerProgress|Struct| |The status of the consumer group. |
|LastTimestamp|Long|1566874931671|The time when the last message consumed by the consumer group was generated. |
|TopicList|Array of TopicList| |The consumption progress of each topic to which the consumer group is subscribed. |
|TopicList| | | |
|LastTimestamp|Long|1566874931649|The time when the last consumed message in the topic was generated. |
|OffsetList|Array of OffsetList| |The list of consumer offsets in the topic. |
|OffsetList| | | |
|BrokerOffset|Long|9|The maximum consumer offset in the partition of the topic. |
|ConsumerOffset|Long|9|The consumer offset in the partition of the topic. |
|LastTimestamp|Long|1566874931649|The time when the last consumed message in the partition was generated. |
|Partition|Integer|0|The ID of the partition. |
|Topic|String|kafka-test|The name of the topic. |
|TotalDiff|Long|0|The total number of unconsumed messages in the topic. |
|TotalDiff|Long|0|The total number of unconsumed messages in all topics. |
|Message|String|operation success.|The response message. |
|RequestId|String|252820E1-A2E6-45F2-B4C9-1056B8CE\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=GetConsumerProgress
&RegionId=cn-hangzhou
&ConsumerId=kafka-test
&InstanceId=alikafka_pre-cn-mp919o4v****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetConsumerProgressResponse>
      <RequestId>252820E1-A2E6-45F2-B4C9-1056B8CE****</RequestId>
      <Message>operation success.</Message>
      <ConsumerProgress>
            <LastTimestamp>1566874931671</LastTimestamp>
            <TopicList>
                  <TopicList>
                        <OffsetList>
                              <OffsetList>
                                    <Partition>0</Partition>
                                    <ConsumerOffset>9</ConsumerOffset>
                                    <LastTimestamp>1566874931671</LastTimestamp>
                                    <BrokerOffset>9</BrokerOffset>
                              </OffsetList>
                        </OffsetList>
                        <LastTimestamp>1566874931671</LastTimestamp>
                        <TotalDiff>0</TotalDiff>
                        <Topic>kafka_test</Topic>
                  </TopicList>
            </TopicList>
            <TotalDiff>0</TotalDiff>
      </ConsumerProgress>
      <Code>200</Code>
      <Success>true</Success>
</GetConsumerProgressResponse>
```

`JSON` format

```
{
    "RequestId": "252820E1-A2E6-45F2-B4C9-1056B8CE****",
    "Message": "operation success.",
    "ConsumerProgress": {
        "LastTimestamp": 1566874931671,
        "TopicList": {
            "TopicList": [
				{
					"OffsetList": {
						"OffsetList": [
							{
								"Partition": 0,
								"ConsumerOffset": 9,
								"LastTimestamp": 1566874931671,
								"BrokerOffset": 9
							}
						]
					},
					"LastTimestamp": 1566874931671,
					"TotalDiff": 0,
					"Topic": "kafka_test"
				}
            ]
        },
        "TotalDiff": 0
    },
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

