# GetTopicStatus {#doc_api_alikafka_GetTopicStatus .reference}

调用 GetTopicStatus 获取 Topic 的消息收发数据。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetTopicStatus&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTopicStatus|要执行的动作。取值：**GetTopicStatus**

 |
|InstanceId|String|是|alikafka\_pre-cn-v0h15tjmo003|实例 ID。

 可调用 [GetInstanceList](https://help.aliyun.com/document_detail/94533.html)获取。

 |
|Topic|String|是|normal\_topic\_9d034262835916103455551be06cc2dc\_6|Topic 名称。

 可调用 [GetTopicList](https://help.aliyun.com/document_detail/94533.html?spm=a2c4g.11186623.2.11.5e374db4Cw7zT7#concept-94533-zh) 获取。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态吗。返回 **200** 代表成功。

 |
|Message|String|operation success.|返回信息。

 |
|RequestId|String|E475C7E2-8C35-46EF-BE7D-5D2A9F5D\*\*\*\*|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |
|TopicStatus| | |Topic 状态。

 |
|LastTimeStamp|Long|1566470063575|最后一条被消费的消息的产生时间。

 |
|OffsetTable| | |偏移列表。

 |
|LastUpdateTimestamp|Long|1566470063547|最后被消费的消息的产生时间。

 |
|MaxOffset|Long|76|消息最大位点。

 |
|MinOffset|Long|0|消息最小位点。

 |
|Partition|Integer|0|分区 ID。

 |
|Topic|String|testkafka|Topic 名称。

 |
|TotalCount|Long|423|消息总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetTopicStatus
&InstanceId=alikafka_pre-cn-v0h15tjmo003 
&Topic=normal_topic_9d034262835916103455551be06cc2dc_6
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetTopicStatusResponse>
	  <Message>operation success.</Message>
	  <RequestId>E475C7E2-8C35-46EF-BE7D-5D2A9F5D****</RequestId>
	  <TopicStatus>
		    <TotalCount>423</TotalCount>
		    <LastTimeStamp>1566470063575</LastTimeStamp>
		    <OffsetTable>
			      <OffsetTable>
				        <Partition>0</Partition>
				        <LastUpdateTimestamp>1566470063547</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>76</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
			      <OffsetTable>
				        <Partition>1</Partition>
				        <LastUpdateTimestamp>1566470063575</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>69</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
			      <OffsetTable>
				        <Partition>2</Partition>
				        <LastUpdateTimestamp>1566470063554</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>70</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
			      <OffsetTable>
				        <Partition>3</Partition>
				        <LastUpdateTimestamp>1566470063538</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>67</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
			      <OffsetTable>
				        <Partition>4</Partition>
				        <LastUpdateTimestamp>1566470063568</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>67</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
			      <OffsetTable>
				        <Partition>5</Partition>
				        <LastUpdateTimestamp>1566470063561</LastUpdateTimestamp>
				        <Topic>testkafka</Topic>
				        <MaxOffset>74</MaxOffset>
				        <MinOffset>0</MinOffset>
			      </OffsetTable>
		    </OffsetTable>
	  </TopicStatus>
	  <Success>true</Success>
	  <Code>200</Code>
</GetTopicStatusResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success.",
	"RequestId":"E475C7E2-8C35-46EF-BE7D-5D2A9F5D****",
	"TopicStatus":{
		"TotalCount":423,
		"OffsetTable":{
			"OffsetTable":[
				{
					"Partition":0,
					"LastUpdateTimestamp":1566470063547,
					"Topic":"testkafka",
					"MaxOffset":76,
					"MinOffset":0
				},
				{
					"Partition":1,
					"LastUpdateTimestamp":1566470063575,
					"Topic":"testkafka",
					"MaxOffset":69,
					"MinOffset":0
				},
				{
					"Partition":2,
					"LastUpdateTimestamp":1566470063554,
					"Topic":"testkafka",
					"MaxOffset":70,
					"MinOffset":0
				},
				{
					"Partition":3,
					"LastUpdateTimestamp":1566470063538,
					"Topic":"testkafka",
					"MaxOffset":67,
					"MinOffset":0
				},
				{
					"Partition":4,
					"LastUpdateTimestamp":1566470063568,
					"Topic":"testkafka",
					"MaxOffset":67,
					"MinOffset":0
				},
				{
					"Partition":5,
					"LastUpdateTimestamp":1566470063561,
					"Topic":"testkafka",
					"MaxOffset":74,
					"MinOffset":0
				}
			]
		},
		"LastTimeStamp":1566470063575
	},
	"Success":true,
	"Code":200
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

