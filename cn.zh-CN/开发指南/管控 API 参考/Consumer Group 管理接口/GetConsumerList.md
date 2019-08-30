# GetConsumerList {#doc_api_alikafka_GetConsumerList .reference}

调用 GetConsumerList 获取 Consumer Group 列表。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetConsumerList&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetConsumerList|要执行的动作。取值：**GetConsumerList**

 |
|InstanceId|String|是|alikafka\_post-cn-v0h18sav0001|实例 ID。

 |
|RegionId|String|是|cn-hangzhou|地域 ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回 **200** 代表成功。

 |
|ConsumerList| | |Consumer Group 列表。

 |
|ConsumerId|String|CID\_c34a6f44915f80d70cb42c4b14ee40c3\_4|Consumer Group 名称。

 |
|InstanceId|String|alikafka\_post-cn-v0h18sav0001|实例 ID。

 |
|RegionId|String|cn-hangzhou|地域 ID。

 |
|Message|String|operation success.|返回信息。

 |
|RequestId|String|808F042B-CB9A-4FBC-9009-00E7DDB64B8C|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetConsumerList
&InstanceId=alikafka_post-cn-v0h18sav0001
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetConsumerListResponse>
      <ConsumerList>
            <ConsumerVO>
                  <ConsumerId>CID_c34a6f44915f80d70cb42c4b14ee40c3_4</ConsumerId>
                  <InstanceId>alikafka_post-cn-v0h18sav0001</InstanceId>
                  <RegionId>cn-hangzhou</RegionId>
            </ConsumerVO>
            <ConsumerVO>
                  <ConsumerId>CID_c34a6f44915f80d70cb42c4b14ee40c3_3</ConsumerId>
                  <InstanceId>alikafka_post-cn-v0h18sav0001</InstanceId>
                  <RegionId>cn-hangzhou</RegionId>
            </ConsumerVO>
      </ConsumerList>
      <Message>operation success.</Message>
      <RequestId>808F042B-CB9A-4FBC-9009-00E7DDB64B8C</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</GetConsumerListResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"ConsumerList":{
		"ConsumerVO":[
			{
				"ConsumerId":"CID_c34a6f44915f80d70cb42c4b14ee40c3_4",
				"RegionId":"cn-hangzhou",
				"InstanceId":"alikafka_post-cn-v0h18sav0001"
			},
			{
				"ConsumerId":"CID_c34a6f44915f80d70cb42c4b14ee40c3_3",
				"RegionId":"cn-hangzhou",
				"InstanceId":"alikafka_post-cn-v0h18sav0001"
			}
		]
	},
	"Message":"operation success.",
	"RequestId":"808F042B-CB9A-4FBC-9009-00E7DDB64B8C",
	"Success":true,
	"Code":200
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

