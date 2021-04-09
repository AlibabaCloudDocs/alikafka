# GetTopicList

Queries topics in a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTopicList|The operation that you want to perform. Set the value to

 **GetTopicList**. |
|CurrentPage|String|Yes|1|The number of the page to return. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose topics you want to query. |
|PageSize|String|Yes|10|The number of entries to return on each page. |
|RegionId|String|No|cn-hangzhou|The ID of the region where the instance resides. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. If 200 is returned, the request is successful. |
|CurrentPage|Integer|1|The number of the page returned. |
|Message|String|operation success.|The response message. |
|PageSize|Integer|10|The number of entries returned per page. |
|RequestId|String|C0D3DC5B-5C37-47AD-9F22-1F5598809\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |
|TopicList|Array of TopicVO| |The information about the topics. |
|TopicVO| | | |
|CreateTime|Long|1576563109000|The time when the topic was created. |
|InstanceId|String|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the instance. |
|RegionId|String|cn-hangzhou|The ID of the region where the instance resides. |
|Remark|String|test|The description of the topic. The description follows these rules:

 -   The value contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The value is 3 to 64 characters in length. |
|Status|Integer|0|The service status of the topic. Valid values:

 **0:** The topic is running.

 A deleted topic does not have a service status. |
|StatusName|String|Running|The name of the service status of the topic. Valid values:

 **Running**

 A deleted topic does not have a service status name. |
|Tags|Array of TagVO| |The tags attached to the topic. |
|TagVO| | | |
|Key|String|Test|The key of the tag. |
|Value|String|Test|The value of the tag. |
|Topic|String|TopicPartitionNum|The name of the topic. The topic name follows these rules:

 -   The name contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name is 3 to 64 characters in length. Names that contain more than 64 characters are automatically truncated.
-   The name cannot be modified after the topic is created. |
|Total|Integer|1|The total number of topics. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=GetTopicList
&InstanceId=alikafka_pre-cn-0pp1954n****
&CurrentPage=1
&PageSize=10
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetTopicListResponse>
      <RequestId>C0D3DC5B-5C37-47AD-9F22-1F5598809***</RequestId>
      <Message>operation success.</Message>
      <PageSize>10</PageSize>
      <CurrentPage>1</CurrentPage>
      <Total>1</Total>
      <TopicList>
            <TopicVO>
                  <Status>0</Status>
                  <PartitionNum>12</PartitionNum>
                  <CompactTopic>false</CompactTopic>
                  <InstanceId>alikafka_pre-cn-0pp1954n****</InstanceId>
                  <CreateTime>1614585068000</CreateTime>
                  <StatusName>Running</StatusName>
                  <RegionId>cn-hangzhou</RegionId>
                  <Topic>TopicPartitionNum</Topic>
                  <LocalTopic>false</LocalTopic>
                  <Tags>
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
  "RequestId": "C0D3DC5B-5C37-47AD-9F22-1F5598809***",
  "Message": "operation success.",
  "PageSize": 10,
  "CurrentPage": 1,
  "Total": 1,
  "TopicList": {
    "TopicVO": [
      {
        "Status": 0,
        "PartitionNum": 12,
        "CompactTopic": false,
        "InstanceId": "alikafka_pre-cn-0pp1954n****",
        "CreateTime": 1614585068000,
        "StatusName": "Running",
        "RegionId": "cn-hangzhou",
        "Topic": "TopicPartitionNum",
        "LocalTopic": false,
        "Tags": {
          "TagVO": []
        },
        "Remark": "test"
      }
    ]
  },
  "Code": 200,
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

