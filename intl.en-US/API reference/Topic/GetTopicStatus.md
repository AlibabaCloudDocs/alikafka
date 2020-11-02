# GetTopicStatus

You can call this operation to query the message sending and receiving status of a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicStatus&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTopicStatus|The operation that you want to perform. Set the value to GetTopicStatus.|
|InstanceId|String|Yes|alikafka\_pre-cn-v0h15tjmo003|The ID of the Message Queue for Apache Kafka instance where the topic is located. |
|Topic|String|Yes|normal\_topic\_9d034262835916103455551be06cc2dc\_6|The name of the topic. |
|RegionId|String|No|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where the topic is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "**200**" is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|E475C7E2-8C35-46EF-BE7D-5D2A9F5D\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |
|TopicStatus|Struct| |The message sending and receiving status of the topic. |
|LastTimeStamp|Long|1566470063575|The time when the last consumed message was generated. |
|OffsetTable|Array| |The returned list of consumer offsets in the topic. |
|OffsetTable| | | |
|LastUpdateTimestamp|Long|1566470063547|The last time when the consumer offset in the topic was updated. |
|MaxOffset|Long|76|The maximum consumer offset in the current partition of the topic. |
|MinOffset|Long|0|The minimum consumer offset in the current partition of the topic. |
|Partition|Integer|0|The ID of the partition in the topic. |
|Topic|String|testkafka|The name of the topic. |
|TotalCount|Long|423|The total number of messages in the topic. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetTopicStatus
&InstanceId=alikafka_pre-cn-v0h15tjmo003
&Topic=normal_topic_9d034262835916103455551be06cc2dc_6
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success. </Message>
<RequestId>D2FF4C1A-2A4A-4C24-BFCD-A2FF2DC1AAFE</RequestId>
<TopicStatus>
    <TotalCount>0</TotalCount>
    <LastTimeStamp>0</LastTimeStamp>
    <OffsetTable>
        <OffsetTable>
            <Partition>0</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>1</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>2</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>3</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>4</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>5</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>6</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>7</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>8</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>9</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>10</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
        <OffsetTable>
            <Partition>11</Partition>
            <LastUpdateTimestamp>0</LastUpdateTimestamp>
            <Topic>demo</Topic>
            <MaxOffset>0</MaxOffset>
            <MinOffset>0</MinOffset>
        </OffsetTable>
    </OffsetTable>
</TopicStatus>
<Success>true</Success>
<Code>200</Code>
```

`JSON` format

```
{
    "Message": "operation success.",
    "RequestId": "D2FF4C1A-2A4A-4C24-BFCD-A2FF2DC1AAFE",
    "TopicStatus": {
        "TotalCount": 0,
        "LastTimeStamp": 0,
        "OffsetTable": {
            "OffsetTable": [
                {
                    "Partition": 0,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 1,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 2,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 3,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 4,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 5,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 6,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 7,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 8,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 9,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 10,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                },
                {
                    "Partition": 11,
                    "LastUpdateTimestamp": 0,
                    "Topic": "demo",
                    "MaxOffset": 0,
                    "MinOffset": 0
                }
            ]
        }
    },
    "Success": true,
    "Code": 200
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

