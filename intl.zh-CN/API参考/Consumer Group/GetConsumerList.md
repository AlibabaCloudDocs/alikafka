# GetConsumerList

调用GetConsumerList获取Consumer Group信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetConsumerList&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetConsumerList|要执行的操作。取值：

 **GetConsumerList**。 |
|InstanceId|String|是|alikafka\_post-cn-v0h18sav\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|ConsumerList|Array of ConsumerVO| |Consumer Group列表。 |
|ConsumerVO| | | |
|ConsumerId|String|CID\_c34a6f44915f80d70cb42c4b14\*\*\*|Consumer Group名称。 |
|InstanceId|String|alikafka\_post-cn-v0h18sav\*\*\*\*|实例ID。 |
|RegionId|String|cn-hangzhou|地域ID。 |
|Remark|String|test|备注。 |
|Tags|Array of TagVO| |标签。 |
|TagVO| | | |
|Key|String|test|标签键。 |
|Value|String|test|标签值。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|808F042B-CB9A-4FBC-9009-00E7DDB6\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetConsumerList
&InstanceId=alikafka_post-cn-v0h18sav****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<GetConsumerList>
      <RequestId>808F042B-CB9A-4FBC-9009-00E7DDB6****</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <ConsumerList>
            <ConsumerVO>
                  <InstanceId>alikafka_post-cn-v0h18sav****</InstanceId>
                  <ConsumerId>CID_c34a6f44915f80d70cb42c4b14***</ConsumerId>
                  <RegionId>cn-hangzhou</RegionId>
                  <Remark>test</Remark>
            </ConsumerVO>
            <ConsumerVO>
                  <Tags>
                        <TagVO>
                              <Value>test</Value>
                              <Key>test</Key>
                        </TagVO>
                  </Tags>
            </ConsumerVO>
      </ConsumerList>
      <Success>true</Success>
</GetConsumerList>
```

`JSON` 格式

```
{
    "RequestId": "808F042B-CB9A-4FBC-9009-00E7DDB6****",
    "Message": "operation success.",
    "Code": 200,
    "ConsumerList": {
        "ConsumerVO": [
            {
                "InstanceId": "alikafka_post-cn-v0h18sav****",
                "ConsumerId": "CID_c34a6f44915f80d70cb42c4b14***",
                "RegionId": "cn-hangzhou",
                "Remark": "test"
            },
            {
                "Tags": {
                    "TagVO": {
                        "Value": "test",
                        "Key": "test"
                    }
                }
            }
        ]
    },
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

