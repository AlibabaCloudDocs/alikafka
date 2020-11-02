# GetConsumerProgress

调用GetConsumerProgress查询Consumer Group的消费状态。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetConsumerProgress&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetConsumerProgress|要执行的操作。取值：

 **GetConsumerProgress**。 |
|ConsumerId|String|是|kafka-test|Consumer Group的名称。 |
|InstanceId|String|是|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|ConsumerProgress|Struct| |消费状态。 |
|LastTimestamp|Long|1566874931671|此Consumer Group最后被消费的消息的产生时间。 |
|TopicList|Array| |此Consumer Group对应的每个Topic的消费进度列表。 |
|TopicList| | | |
|LastTimestamp|Long|1566874931649|该Topic最后被消费的消息的产生时间。 |
|OffsetList|Array| |偏移列表。 |
|OffsetList| | | |
|BrokerOffset|Long|9|最大位点。 |
|ConsumerOffset|Long|9|消费位点。 |
|LastTimestamp|Long|1566874931649|该分区最后被消费的消息的产生时间。 |
|Partition|Integer|0|分区ID。 |
|Topic|String|kafka-test|Topic名称。 |
|TotalDiff|Long|0|该Topic的未消费消息总量，即堆积量。 |
|TotalDiff|Long|0|所有Topic的未消费消息总量，即堆积量。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|252820E1-A2E6-45F2-B4C9-1056B8CE\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetConsumerProgress
&ConsumerId=kafka-test
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<GetConsumerProgressResponse>
      <RequestId>252820E1-A2E6-45F2-B4C9-1056B8CE****</RequestId>
      <Message>operation success.</Message>
      <ConsumerProgress>
            <LastTimestamp>1566874931671</LastTimestamp>
            <TopicList>
                  <TopicList>
                        <LastTimestamp>1566874931649</LastTimestamp>
                        <TotalDiff>0</TotalDiff>
                        <Topic>kafka-test</Topic>
                  </TopicList>
                  <TopicList>
                        <OffsetList>
                              <OffsetList>
                                    <Partition>0</Partition>
                                    <ConsumerOffset>9</ConsumerOffset>
                                    <LastTimestamp>1566874931649</LastTimestamp>
                                    <BrokerOffset>9</BrokerOffset>
                              </OffsetList>
                        </OffsetList>
                  </TopicList>
            </TopicList>
            <TotalDiff>0</TotalDiff>
      </ConsumerProgress>
      <Code>200</Code>
      <Success>true</Success>
</GetConsumerProgressResponse>
```

`JSON` 格式

```
{
    "RequestId": "252820E1-A2E6-45F2-B4C9-1056B8CE****",
    "Message": "operation success.",
    "ConsumerProgress": {
        "LastTimestamp": 1566874931671,
        "TopicList": {
            "TopicList": [
                {
                    "LastTimestamp": 1566874931649,
                    "TotalDiff": 0,
                    "Topic": "kafka-test"
                },
                {
                    "OffsetList": {
                        "OffsetList": {
                            "Partition": 0,
                            "ConsumerOffset": 9,
                            "LastTimestamp": 1566874931649,
                            "BrokerOffset": 9
                        }
                    }
                }
            ]
        },
        "TotalDiff": 0
    },
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

