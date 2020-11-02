# GetTopicStatus

Queries the status of a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicStatus&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTopicStatus|The operation that you want to perform. Set the value to

 **GetTopicStatus**. |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h15tjmo003|The ID of the Message Queue for Apache Kafka instance that contains the topic. |
|Topic|String|Yes|normal\_topic\_9d034262835916103455551be06cc2dc\_6|The name of the topic that you want to query. |
|RegionId|String|No|cn-hangzhou|The ID of the region where the instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|E475C7E2-8C35-46EF-BE7D-5D2A9F5D\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |
|TopicStatus|Struct| |The message sending and receiving status of the topic. |
|LastTimeStamp|Long|1566470063575|The time when the last consumed message was generated. |
|OffsetTable|Array| |The list of consumer offsets in the topic. |
|OffsetTable| | | |
|LastUpdateTimestamp|Long|1566470063547|The time when the consumer offset in the topic was last updated. |
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
<GetTopicStatusResponse>
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
</GetTopicStatusResponse>
```

`JSON` format

```
{
    "RequestId":"E475C7E2-8C35-46EF-BE7D-5D2A9F5D****",
    "Message":"operation success.",
    "TopicStatus":{
        "LastTimeStamp":"1566470063575",
        "TotalCount":"423",
        "OffsetTable":{
            "OffsetTable":[
                {
                    "Partition":"0",
                    "MaxOffset":"76",
                    "Topic":"testkafka",
                    "LastUpdateTimestamp":"1566470063547",
                    "MinOffset":"0"
                }
            ]
        }
    },
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

