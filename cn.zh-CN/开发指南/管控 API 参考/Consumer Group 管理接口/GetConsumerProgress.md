# GetConsumerProgress {#doc_api_alikafka_GetConsumerProgress .reference}

调用 GetConsumerProgress 查询 Consumer Group 的消费状态。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetConsumerProgress&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetConsumerProgress|要执行的动作。取值：**GetConsumerProgress**

 |
|ConsumerId|String|是|kafka-test|Consumer Group ID。

 |
|InstanceId|String|是|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例 ID。

 |
|RegionId|String|是|cn-hangzhou|地域 ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回 **200** 代表成功。

 |
|ConsumerProgress| | |消费状态。

 |
|LastTimestamp|Long|1566874931671|此 Consumer Group 最后被消费的消息的产生时间。

 |
|TopicList| | |此 Consumer Group 对应的每个 Topic 的消费进度列表。

 |
|LastTimestamp|Long|1566874931649|该分区最后被消费的消息的产生时间。

 |
|OffsetList| | |偏移列表。

 |
|BrokerOffset|Long|9|最大位点。

 |
|ConsumerOffset|Long|9|消费位点。

 |
|LastTimestamp|Long|1566874931649|该分区最后被消费的消息的产生时间。

 |
|Partition|Integer|0|分区 ID。

 |
|Topic|String|kafka-test|Topic 名称。

 |
|TotalDiff|Long|0|该 Topic 的未消费消息总量，即堆积量。

 |
|TotalDiff|Long|0|此 Consumer Group 未消费的消息总量，即堆积量。

 |
|Message|String|operation success.|返回信息。

 |
|RequestId|String|252820E1-A2E6-45F2-B4C9-1056B8CE\*\*\*\*|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetConsumerProgress
&ConsumerId=kafka-test
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetConsumerProgressResponse>
      <Message>operation success.</Message>
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

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success.",
	"RequestId":"252820E1-A2E6-45F2-B4C9-1056B8CE****",
	"ConsumerProgress":{
		"TotalDiff":0,
		"LastTimestamp":1566874931671,
		"TopicList":{
			"TopicList":[
				{
					"TotalDiff":0,
					"LastTimestamp":1566874931671,
					"OffsetList":{
						"OffsetList":[
							{
								"Partition":0,
								"LastTimestamp":1566874931649,
								"BrokerOffset":9,
								"ConsumerOffset":9
							},
							{
								"Partition":1,
								"LastTimestamp":1566874931605,
								"BrokerOffset":9,
								"ConsumerOffset":9
							},
							{
								"Partition":2,
								"LastTimestamp":1566874931561,
								"BrokerOffset":10,
								"ConsumerOffset":10
							},
							{
								"Partition":3,
								"LastTimestamp":1566874931628,
								"BrokerOffset":8,
								"ConsumerOffset":8
							},
							{
								"Partition":4,
								"LastTimestamp":1566874931579,
								"BrokerOffset":8,
								"ConsumerOffset":8
							},
							{
								"Partition":5,
								"LastTimestamp":1566874931570,
								"BrokerOffset":10,
								"ConsumerOffset":10
							},
							{
								"Partition":6,
								"LastTimestamp":1566874931639,
								"BrokerOffset":9,
								"ConsumerOffset":9
							},
							{
								"Partition":7,
								"LastTimestamp":1566874931586,
								"BrokerOffset":8,
								"ConsumerOffset":8
							},
							{
								"Partition":8,
								"LastTimestamp":1566874931661,
								"BrokerOffset":9,
								"ConsumerOffset":9
							},
							{
								"Partition":9,
								"LastTimestamp":1566874931616,
								"BrokerOffset":8,
								"ConsumerOffset":8
							},
							{
								"Partition":10,
								"LastTimestamp":1566874931596,
								"BrokerOffset":8,
								"ConsumerOffset":8
							},
							{
								"Partition":11,
								"LastTimestamp":1566874931671,
								"BrokerOffset":9,
								"ConsumerOffset":9
							}
						]
					},
					"Topic":"kafka-test"
				}
			]
		}
	},
	"Success":true,
	"Code":200
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

