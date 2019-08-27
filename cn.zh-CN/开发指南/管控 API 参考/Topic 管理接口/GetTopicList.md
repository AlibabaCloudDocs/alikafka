# GetTopicList {#doc_api_alikafka_GetTopicList .reference}

调用 GetTopicList 获取 Topic 的列表。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetTopicList&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetTopicList|要执行的动作。取值：**GetTopicList**

 |
|InstanceId|String|是|alikafka\_pre-cn-0pp1954n2003|实例 ID。

 |
|RegionId|String|否|cn-hangzhou|地域 ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回 **200** 为成功。

 |
|CurrentPage|Integer|1|当前页面。

 |
|Message|String|operation success.|返回信息。

 |
|PageSize|Integer|10000|页面大小。

 |
|RequestId|String|82BD585C-17A1-486E-B3E8-AABCE8EEF67B|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |
|TopicList| | |Topic 列表。

 |
|CompactTopic|Boolean|false|Topic 的日志清理策略是否为 Compact。

 |
|CreateTime|Long|1566804394000|创建时间。

 |
|InstanceId|String|alikafka\_pre-cn-0pp1954n2003|实例 ID。

 |
|LocalTopic|Boolean|false|Topic 的存储引擎是否为 Local。

 |
|PartitionNum|Integer|12|分区数。

 |
|RegionId|String|cn-hangzhou|地域 ID。

 |
|Remark|String|kafka\_test\_topic|Topic 备注。

 |
|Status|Integer|0|状态码。

 -   **0**：服务中。
-   **1**：冻结。
-   **2**：暂停。

 |
|StatusName|String|服务中|状态描述。

 |
|Topic|String|kafka\_test\_topic|Topic 名称。

 |
|Total|Integer|5|Topic 总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetTopicList
&InstanceId=alikafka_pre-cn-0pp1954n2003 
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetTopicListResponse>
      <TopicList>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1562058090000</CreateTime>
                  <Topic>normal_topic_9d034262835916103455551be06cc2dc_6</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1562058088000</CreateTime>
                  <Topic>normal_topic_9d034262835916103455551be06cc2dc_5</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1561364809000</CreateTime>
                  <Topic>rock</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>rock</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>true</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1561105770000</CreateTime>
                  <Topic>connect-status</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>true</LocalTopic>
                  <Remark>connect-status</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>1</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>true</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1561105754000</CreateTime>
                  <Topic>connect-configs</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>true</LocalTopic>
                  <Remark>connect-configs</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>true</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1561105732000</CreateTime>
                  <Topic>connect-offsets</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>true</LocalTopic>
                  <Remark>connect-offsets</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>12</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1561030134000</CreateTime>
                  <Topic>zyhTest</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>xxx</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>12</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1560928862000</CreateTime>
                  <Topic>test</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>1111111111</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>48</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1559751466000</CreateTime>
                  <Topic>compact_topic_9d034262835916103455551be06cc2dc_2</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>48</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1559751462000</CreateTime>
                  <Topic>compact_topic_9d034262835916103455551be06cc2dc_1</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1559751457000</CreateTime>
                  <Topic>normal_topic_9d034262835916103455551be06cc2dc_4</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
            <TopicVO>
                  <PartitionNum>24</PartitionNum>
                  <Status>0</Status>
                  <CompactTopic>false</CompactTopic>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-v0h15tjmo003</InstanceId>
                  <CreateTime>1559751456000</CreateTime>
                  <Topic>normal_topic_9d034262835916103455551be06cc2dc_3</Topic>
                  <StatusName>服务中</StatusName>
                  <LocalTopic>false</LocalTopic>
                  <Remark>topic1</Remark>
            </TopicVO>
      </TopicList>
      <Message>operation success.</Message>
      <PageSize>10000</PageSize>
      <RequestId>44DFF887-ACEC-4DF5-BE10-6E0E23A3A953</RequestId>
      <CurrentPage>1</CurrentPage>
      <Success>true</Success>
      <Code>200</Code>
      <Total>12</Total>
</GetTopicListResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"TopicList":{
		"TopicVO":[
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1562058090000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"normal_topic_9d034262835916103455551be06cc2dc_6",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1562058088000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"normal_topic_9d034262835916103455551be06cc2dc_5",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1561364809000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"rock",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"rock"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":true,
				"CreateTime":1561105770000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"connect-status",
				"StatusName":"服务中",
				"LocalTopic":true,
				"Remark":"connect-status"
			},
			{
				"PartitionNum":1,
				"Status":0,
				"CompactTopic":true,
				"CreateTime":1561105754000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"connect-configs",
				"StatusName":"服务中",
				"LocalTopic":true,
				"Remark":"connect-configs"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":true,
				"CreateTime":1561105732000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"connect-offsets",
				"StatusName":"服务中",
				"LocalTopic":true,
				"Remark":"connect-offsets"
			},
			{
				"PartitionNum":12,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1561030134000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"zyhTest",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"xxx"
			},
			{
				"PartitionNum":12,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1560928862000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"test",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"1111111111"
			},
			{
				"PartitionNum":48,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1559751466000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"compact_topic_9d034262835916103455551be06cc2dc_2",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			},
			{
				"PartitionNum":48,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1559751462000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"compact_topic_9d034262835916103455551be06cc2dc_1",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1559751457000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"normal_topic_9d034262835916103455551be06cc2dc_4",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			},
			{
				"PartitionNum":24,
				"Status":0,
				"CompactTopic":false,
				"CreateTime":1559751456000,
				"InstanceId":"alikafka_pre-cn-v0h15tjmo003",
				"RegionId":"cn-hangzhou",
				"Topic":"normal_topic_9d034262835916103455551be06cc2dc_3",
				"StatusName":"服务中",
				"LocalTopic":false,
				"Remark":"topic1"
			}
		]
	},
	"Message":"operation success.",
	"PageSize":10000,
	"RequestId":"44DFF887-ACEC-4DF5-BE10-6E0E23A3A953",
	"Success":true,
	"CurrentPage":1,
	"Code":200,
	"Total":12
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|500|InternalError|An internal error occurred; please try again later.|系统内部错误，请稍后重试|

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

