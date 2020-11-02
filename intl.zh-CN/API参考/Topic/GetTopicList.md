# GetTopicList

调用GetTopicList获取Topic信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTopicList|要执行的操作。取值：

 **GetTopicList**。 |
|CurrentPage|String|是|1|当前页。 |
|InstanceId|String|是|alikafka\_pre-cn-0pp1954n\*\*\*\*|实例的ID。 |
|PageSize|String|是|10|页大小。 |
|RegionId|String|否|cn-hangzhou|实例的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|CurrentPage|Integer|1|当前页面。 |
|Message|String|operation success.|返回信息。 |
|PageSize|Integer|10|页面大小。 |
|RequestId|String|C0D3DC5B-5C37-47AD-9F22-1F5598809\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |
|TopicList|Array| |Topic详情。 |
|TopicVO| | | |
|CreateTime|Long|1576563109000|创建时间。 |
|InstanceId|String|alikafka\_pre-cn-0pp1954n\*\*\*\*|实例ID。 |
|RegionId|String|cn-hangzhou|实例的地域ID。 |
|Remark|String|test|备注。取值：

 -   只能包含字母、数字、下划线（\_）、短划线（-）。
-   长度为3~64字符。 |
|Status|Integer|0|服务状态。取值：

 **0**：服务中

 删除Topic，则Topic无服务状态。 |
|StatusName|String|服务中|服务状态名称。取值：

 **服务中**。

 删除Topic，则Topic无服务状态名称。 |
|Tags|Array| |标签。 |
|TagVO| | | |
|Key|String|Test|标签键。 |
|Value|String|Test|标签值。 |
|Topic|String|TopicPartitionNum|Topic名称。取值：

 -   只能包含字母、数字、下划线（\_）和短划线（-）。
-   限制在3~64字符，长于64字符将被自动截取。
-   一旦创建，将无法修改。 |
|Total|Integer|1|Topic总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetTopicList
&CurrentPage=1
&InstanceId=alikafka_pre-cn-0pp1954n2003
&PageSize=10
&<公共请求参数>
```

正常返回示例

`XML` 格式

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
                  <PartitionNum>6</PartitionNum>
                  <CompactTopic>false</CompactTopic>
                  <InstanceId>alikafka_pre-cn-0pp1954n****</InstanceId>
                  <CreateTime>1586260357000</CreateTime>
                  <StatusName>服务中</StatusName>
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

`JSON` 格式

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
                "StatusName":"服务中",
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

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

