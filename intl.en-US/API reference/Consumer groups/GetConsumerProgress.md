# GetConsumerProgress

You can call this operation to query the consumption status of a consumer group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetConsumerProgress&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetConsumerProgress|The operation that you want to perform. Set the value to GetConsumerProgress. |
|ConsumerId|String|Yes|kafka-test|The name of the consumer group. |
|InstanceId|String|Yes|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where the consumer group is located. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where the consumer group is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|ConsumerProgress|Struct| |The consumption status of the consumer group. |
|LastTimestamp|Long|1566874931671|The time when the last message consumed by the consumer group was generated. |
|TopicList|Array| |The consumption progress of each topic subscribed by the consumer group. |
|TopicList| | | |
|LastTimestamp|Long|1566874931649|The time when the last consumed message in the topic was generated. |
|OffsetList|Array| |The returned list of consumer offsets in the topic. |
|OffsetList| | | |
|BrokerOffset|Long|9|The maximum consumer offset in the partition of the topic. |
|ConsumerOffset|Long|9|The consumer offset in the partition of the topic. |
|LastTimestamp|Long|1566874931649|The time when the last consumed message in the partition of the topic was generated. |
|Partition|Integer|0|The ID of the partition in the topic. |
|Topic|String|kafka-test|The name of the topic. |
|TotalDiff|Long|0|The total number of unconsumed messages, or in other words, accumulated messages, of the topic subscribed by the consumer group. |
|TotalDiff|Long|0|The total number of unconsumed messages, or in other words, accumulated messages, of all topics subscribed by the consumer group. |
|Message|String|operation success.|The returned message. |
|RequestId|String|252820E1-A2E6-45F2-B4C9-1056B8CE\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetConsumerProgress
&ConsumerId=kafka-test
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetConsumerProgressResponse>
        <Message>operation success. </Message>
        <RequestId>252820E1-A2E6-45F2-B4C9-1056B8CE****</RequestId>
        <ConsumerProgress>
                <TotalDiff>0</TotalDiff>
                <LastTimestamp>1566874931671</LastTimestamp>
                <TopicList>
                        <TopicList>
                                <TotalDiff>0</TotalDiff>
                                <OffsetList>
                                        <OffsetList>
                                                <Partition>0</Partition>
                                                <LastTimestamp>1566874931649</LastTimestamp>
                                                <BrokerOffset>9</BrokerOffset>
                                                <ConsumerOffset>9</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>1</Partition>
                                                <LastTimestamp>1566874931605</LastTimestamp>
                                                <BrokerOffset>9</BrokerOffset>
                                                <ConsumerOffset>9</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>2</Partition>
                                                <LastTimestamp>1566874931561</LastTimestamp>
                                                <BrokerOffset>10</BrokerOffset>
                                                <ConsumerOffset>10</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>3</Partition>
                                                <LastTimestamp>1566874931628</LastTimestamp>
                                                <BrokerOffset>8</BrokerOffset>
                                                <ConsumerOffset>8</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>4</Partition>
                                                <LastTimestamp>1566874931579</LastTimestamp>
                                                <BrokerOffset>8</BrokerOffset>
                                                <ConsumerOffset>8</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>5</Partition>
                                                <LastTimestamp>1566874931570</LastTimestamp>
                                                <BrokerOffset>10</BrokerOffset>
                                                <ConsumerOffset>10</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>6</Partition>
                                                <LastTimestamp>1566874931639</LastTimestamp>
                                                <BrokerOffset>9</BrokerOffset>
                                                <ConsumerOffset>9</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>7</Partition>
                                                <LastTimestamp>1566874931586</LastTimestamp>
                                                <BrokerOffset>8</BrokerOffset>
                                                <ConsumerOffset>8</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>8</Partition>
                                                <LastTimestamp>1566874931661</LastTimestamp>
                                                <BrokerOffset>9</BrokerOffset>
                                                <ConsumerOffset>9</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>9</Partition>
                                                <LastTimestamp>1566874931616</LastTimestamp>
                                                <BrokerOffset>8</BrokerOffset>
                                                <ConsumerOffset>8</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>10</Partition>
                                                <LastTimestamp>1566874931596</LastTimestamp>
                                                <BrokerOffset>8</BrokerOffset>
                                                <ConsumerOffset>8</ConsumerOffset>
                                        </OffsetList>
                                        <OffsetList>
                                                <Partition>11</Partition>
                                                <LastTimestamp>1566874931671</LastTimestamp>
                                                <BrokerOffset>9</BrokerOffset>
                                                <ConsumerOffset>9</ConsumerOffset>
                                        </OffsetList>
                                </OffsetList>
                                <LastTimestamp>1566874931671</LastTimestamp>
                                <Topic>kafka-test</Topic>
                        </TopicList>
                </TopicList>
        </ConsumerProgress>
        <Success>true</Success>
        <Code>200</Code>
</GetConsumerProgressResponse>
```

`JSON` format

```
{
    "GetConsumerProgressResponse": {
        "Message": "operation success.",
        "RequestId": "252820E1-A2E6-45F2-B4C9-1056B8CE****",
        "ConsumerProgress": {
            "TotalDiff": 0,
            "LastTimestamp": 1566874931671,
            "TopicList": {
                "TopicList": {
                    "TotalDiff": 0,
                    "OffsetList": {
                        "OffsetList": [
                            {
                                "Partition": 0,
                                "LastTimestamp": 1566874931649,
                                "BrokerOffset": 9,
                                "ConsumerOffset": 9
                            },
                            {
                                "Partition": 1,
                                "LastTimestamp": 1566874931605,
                                "BrokerOffset": 9,
                                "ConsumerOffset": 9
                            },
                            {
                                "Partition": 2,
                                "LastTimestamp": 1566874931561,
                                "BrokerOffset": 10,
                                "ConsumerOffset": 10
                            },
                            {
                                "Partition": 3,
                                "LastTimestamp": 1566874931628,
                                "BrokerOffset": 8,
                                "ConsumerOffset": 8
                            },
                            {
                                "Partition": 4,
                                "LastTimestamp": 1566874931579,
                                "BrokerOffset": 8,
                                "ConsumerOffset": 8
                            },
                            {
                                "Partition": 5,
                                "LastTimestamp": 1566874931570,
                                "BrokerOffset": 10,
                                "ConsumerOffset": 10
                            },
                            {
                                "Partition": 6,
                                "LastTimestamp": 1566874931639,
                                "BrokerOffset": 9,
                                "ConsumerOffset": 9
                            },
                            {
                                "Partition": 7,
                                "LastTimestamp": 1566874931586,
                                "BrokerOffset": 8,
                                "ConsumerOffset": 8
                            },
                            {
                                "Partition": 8,
                                "LastTimestamp": 1566874931661,
                                "BrokerOffset": 9,
                                "ConsumerOffset": 9
                            },
                            {
                                "Partition": 9,
                                "LastTimestamp": 1566874931616,
                                "BrokerOffset": 8,
                                "ConsumerOffset": 8
                            },
                            {
                                "Partition": 10,
                                "LastTimestamp": 1566874931596,
                                "BrokerOffset": 8,
                                "ConsumerOffset": 8
                            },
                            {
                                "Partition": 11,
                                "LastTimestamp": 1566874931671,
                                "BrokerOffset": 9,
                                "ConsumerOffset": 9
                            }
                        ]
                    },
                    "LastTimestamp": 1566874931671,
                    "Topic": "kafka-test"
                }
            }
        },
        "Success": true,
        "Code": 200
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

