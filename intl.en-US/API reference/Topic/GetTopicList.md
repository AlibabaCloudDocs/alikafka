# GetTopicList

Queries the topics on a Message Queue for Apache Kafka instance.

****

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTopicList|The operation that you want to perform. Set the value to

 **GetTopicList**. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose topics you want to query. |
|CurrentPage|String|No|1|The number of the page to return. |
|PageSize|String|No|10|The number of entries to return on each page. |
|RegionId|String|No|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. The HTTP status code 200 indicates that the request is successful. |
|CurrentPage|Integer|1|The page number of the returned page. |
|PageSize|Integer|10|The number of entries returned per page. |
|Message|String|operation success.|The returned message. |
|RequestId|String|C0D3DC5B-5C37-47AD-9F22-1F5598809\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |
|TopicList|Array of TopicVO| |The information about the topics. |
|TopicVO| | | |
|CompactTopic|Boolean|false|The log cleanup policy for the topic. This parameter is returned when the Local Storage mode is specified for the topic. Valid values:

 -   false: The default log cleanup policy is used.
-   true: The Apache Kafka log compaction policy is used. |
|CreateTime|Long|1576563109000|The time when the topic was created. |
|InstanceId|String|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|LocalTopic|Boolean|false|The storage engine of the topic. Valid values:

 -   false: the Cloud Storage mode.
-   true: the Local Storage mode. |
|PartitionNum|Integer|6|The number of partitions in the topic. |
|RegionId|String|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance. |
|Remark|String|test|The description of the topic. The description follows these rules:

 -   The description contains only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The description is 3 to 64 characters in length. |
|Status|Integer|0|The status of the topic. Valid values:

 **0:** The topic is running.

 A deleted topic does not have a status. |
|StatusName|String|Running|The name of the status of the topic. Valid values:

 **Running**

 A deleted topic does not have a status name. |
|Tags|Array of TagVO| |The tags attached to the topic. |
|TagVO| | | |
|Key|String|Test|The key of the tag. |
|Value|String|Test|The value of the tag |
|Topic|String|TopicPartitionNum|The name of the topic. The topic name follows these rules:

 -   The name contains only letters, digits, hyphens \(-\), and underscores \(\_\).
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
  <RequestId>C81688BE-F2B1-499F-B9D3-61CDA471F2C8</RequestId>
  <Message>operation success.</Message>
  <PageSize>10000</PageSize>
  <CurrentPage>1</CurrentPage>
  <Total>15</Total>
  <TopicList>
        <TopicVO>
              <Status>0</Status>
              <PartitionNum>6</PartitionNum>
              <CompactTopic>false</CompactTopic>
              <InstanceId>alikafka_post-cn-st21xhct6001</InstanceId>
              <CreateTime>1618303927000</CreateTime>
              <StatusName>Running</StatusName>
              <RegionId>cn-hangzhou</RegionId>
              <Topic>connect-error-nianxu-kafka2fc-test</Topic>
              <LocalTopic>false</LocalTopic>
              <Tags>
                    <TagVO>
                          <Value>test</Value>
                          <Key>test</Key>
                    </TagVO>
              </Tags>
              <Remark>create_by_kafka_connector_do_not_delete</Remark>
        </TopicVO>
  </TopicList>
  <Code>200</Code>
  <Success>true</Success>
</GetTopicListResponse>
```

`JSON` format

```
{
  "RequestId": "C81688BE-F2B1-499F-B9D3-61CDA471F2C8",
  "Message": "operation success.",
  "PageSize": 10000,
  "CurrentPage": 1,
  "Total": 15,
  "TopicList": {
    "TopicVO": [
      {
        "Status": 0,
        "PartitionNum": 6,
        "CompactTopic": false,
        "InstanceId": "alikafka_post-cn-st21xhct6001",
        "CreateTime": 1618303927000,
        "StatusName": "Running",
        "RegionId": "cn-hangzhou",
        "Topic": "connect-error-nianxu-kafka2fc-test",
        "LocalTopic": false,
        "Tags": {
          "TagVO": [
            {
              "Value": "test",
              "Key": "test"
            }
          ]
        },
        "Remark": "create_by_kafka_connector_do_not_delete"
      }
    ]
  },
  "Code": 200,
  "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

