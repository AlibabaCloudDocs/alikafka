# GetTopicList

Queries topics on a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|GetTopicList|The operation that you want to perform. Set the value to

 **GetTopicList**. |
|CurrentPage|String|Yes|1|The page to return. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the instance in which you want to query topics. |
|PageSize|String|Yes|10|The number of entries to return on each page. |
|RegionId|String|No|cn-hangzhou|The ID of the region where the instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|CurrentPage|Integer|1|The page that is returned. |
|Message|String|operation success.|The response message. |
|PageSize|Integer|10|The number of entries returned per page. |
|RequestId|String|C0D3DC5B-5C37-47AD-9F22-1F5598809\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |
|TopicList|Array| |Information about the topics. |
|TopicVO| | | |
|CreateTime|Long|1576563109000|The time when the instance was created. |
|InstanceId|String|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the instance that was queried. |
|RegionId|String|cn-hangzhou|The ID of the region where the instance is located. |
|Remark|String|test|The description of the topic. Valid values:

 -   The value can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The value must be 3 to 64 characters in length. |
|Status|Integer|0|The service status of the topic. Valid values:

 **0:** The topic is running.

 A deleted topic does not have a service status. |
|StatusName|String|Running|The name of the service status of the topic. Valid values:

 **Running**

 A deleted topic does not have a service status name. |
|Tags|Array| |The tags bound to the topic. |
|TagVO| | | |
|Key|String|Test|The key of the resource tag. |
|Value|String|Test|The value of the resource tag. |
|Topic|String|TopicPartitionNum|The name of the topic. Valid values:

 -   The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length. Names that contain more than 64 characters will be automatically truncated.
-   The name cannot be modified after the topic is created. |
|Total|Integer|1|The total number of topics. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetTopicList
&CurrentPage=1
&InstanceId=alikafka_pre-cn-0pp1954n2003
&PageSize=10
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetTopicListResponse>
      <RequestId>C0D3DC5B-5C37-47AD-9F22-1F5598809***</RequestId>
      <Message>operation success. </Message>
      <PageSize>10</PageSize>
      <CurrentPage>1</CurrentPage>
      <Total>1</Total>
      <TopicList>
            <TopicVO>
                  <Status>0</Status>
                  <PartitionNum>6</PartitionNum>
                  <CompactTopic>false</CompactTopic>
                  <InstanceId>alikafka_pre-cn-0pp1954n****</InstanceId>
                  <CreateTime>1586260357000</CreateTime>
                  <StatusName>Running</StatusName>
                  <RegionId>cn-hangzhou</RegionId>
                  <Topic>TopicPartitionNum</Topic>
                  <LocalTopic>false</LocalTopic>
                  <Tags>
                        <TagVO>
                              <Value>Test</Value>
                              <Key>Test</Key>
                        </TagVO>
                  </Tags>
                  <Remark>test</Remark>
            </TopicVO>
      </TopicList>
      <Code>200</Code>
      <Success>true</Success>
</GetTopicListResponse>
```

`JSON` format

```
{
    "RequestId":"C0D3DC5B-5C37-47AD-9F22-1F5598809***",
    "Message":"operation success.",
    "PageSize":"10",
    "CurrentPage":"1",
    "Total":"1",
    "TopicList":{
        "TopicVO":[
            {
                "Status":"0",
                "InstanceId":"alikafka_pre-cn-0pp1954n****",
                "CreateTime":"1576563109000",
                "StatusName":"Running",
                "RegionId":"cn-hangzhou",
                "Topic":"TopicPartitionNum",
                "Remark":"test"
            },
            {
                "Tags":{
                    "TagVO":[
                        {
                            "Value":"Test",
                            "Key":"Test"
                        }
                    ]
                }
            }
        ]
    },
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

