# GetTopicList

You can call this operation to query topics.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetTopicList|The operation that you want to perform. Set the value to GetTopicList. |
|CurrentPage|String|Yes|1|The number of the page to return. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose topics you want to query. |
|PageSize|String|Yes|10|The number of entries to return on each page. |
|RegionId|String|No|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance whose topics you want to query. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|CurrentPage|Integer|1|The page number of the returned page. |
|Message|String|operation success.|The returned message. |
|PageSize|Integer|10000|The number of entries returned per page. |
|RequestId|String|82BD585C-17A1-486E-B3E8-AABCE8EE\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |
|TopicList|Array| |The returned list of topics. |
|TopicVO| | | |
|CreateTime|Long|1576563109000|The time when the topic was created. |
|InstanceId|String|alikafka\_pre-cn-0pp1ftnx\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where the topic is located. |
|RegionId|String|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where the topic is located. |
|Remark|String|test2|The description of the topic. The value of this parameter must meet the following requirements:

 -   The value can only contain letters, digits, hyphens \(-\), and underscores \(\_\).
-   The value must be 3 to 64 characters in length. |
|Status|Integer|0|The service status of the topic. Valid value:

 **0:** The topic is running.

 **Note:** If the topic has been deleted, this parameter is not returned. |
|StatusName|String|Running|The name of the service status of the topic. Valid value:

 **Running**

 **Note:** If the topic has been deleted, this parameter is not returned. |
|Tags|Array| |The tags bound to the topic. |
|TagVO| | | |
|Key|String|Test|The key of the tag bound to the topic. |
|Value|String|Test|The value of the tag bound to the topic. |
|Topic|String|test2|The name of the topic. The value of this parameter must meet the following requirements:

 -   The name can only contain letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length, and will be automatically truncated if it contains more characters.
-   The name cannot be modified after being created. |
|Total|Integer|2|The total number of returned topics. |

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
<TopicList>
    <TopicVO>
        <PartitionNum>12</PartitionNum>
        <Tags>
        </Tags>
        <Status>0</Status>
        <CompactTopic>false</CompactTopic>
        <RegionId>cn-hangzhou</RegionId>
        <InstanceId>alikafka_pre-cn-0pp1ftnxu00y</InstanceId>
        <CreateTime>1576563109000</CreateTime>
        <Topic>test2</Topic>
        <StatusName>Running</StatusName>
        <LocalTopic>false</LocalTopic>
        <Remark>test</Remark>
    </TopicVO>
    <TopicVO>
        <PartitionNum>12</PartitionNum>
        <Tags>
        </Tags>
        <Status>0</Status>
        <CompactTopic>false</CompactTopic>
        <RegionId>cn-hangzhou</RegionId>
        <InstanceId>alikafka_pre-cn-0pp1ftnxu00y</InstanceId>
        <CreateTime>1576563103000</CreateTime>
        <Topic>test1</Topic>
        <StatusName>Running</StatusName>
        <LocalTopic>false</LocalTopic>
        <Remark>test</Remark>
    </TopicVO>
</TopicList>
<Message>operation success. </Message>
<PageSize>10000</PageSize>
<RequestId>ABBF8EF6-3598-43E4-91D6-2FD211A90075</RequestId>
<CurrentPage>1</CurrentPage>
<Success>true</Success>
<Code>200</Code>
<Total>2</Total>
```

`JSON` format

```
{
    "TopicList": {
        "TopicVO": [
            {
                "PartitionNum": 12,
                "Tags": {
                    "TagVO": []
                },
                "Status": 0,
                "CompactTopic": false,
                "RegionId": "cn-hangzhou",
                "InstanceId": "alikafka_pre-cn-0pp1ftnxu00y",
                "CreateTime": 1576563109000,
                "Topic": "test2",
                "StatusName": "Running",
                "LocalTopic": false,
                "Remark": "test"
            },
            {
                "PartitionNum": 12,
                "Tags": {
                    "TagVO": []
                },
                "Status": 0,
                "CompactTopic": false,
                "RegionId": "cn-hangzhou",
                "InstanceId": "alikafka_pre-cn-0pp1ftnxu00y",
                "CreateTime": 1576563103000,
                "Topic": "test1",
                "StatusName": "Running",
                "LocalTopic": false,
                "Remark": "test"
            }
        ]
    },
    "Message": "operation success.",
    "PageSize": 10000,
    "RequestId": "ABBF8EF6-3598-43E4-91D6-2FD211A90075",
    "CurrentPage": 1,
    "Success": true,
    "Code": 200,
    "Total": 2
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

