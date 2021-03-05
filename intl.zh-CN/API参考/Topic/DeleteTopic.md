# DeleteTopic

调用DeleteTopic删除Topic。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DeleteTopic&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteTopic|要执行的操作。取值：

 **DeleteTopic**。 |
|InstanceId|String|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|Topic|String|是|test|Topic名称。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|请求的ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DeleteTopic
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&Topic=test
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<DeleteTopicResponse>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <Success>true</Success>
</DeleteTopicResponse>
```

`JSON`格式

```
{
    "RequestId": "06084011-E093-46F3-A51F-4B19A8AD****",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

