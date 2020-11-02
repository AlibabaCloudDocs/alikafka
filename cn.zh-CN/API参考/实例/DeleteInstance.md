# DeleteInstance

调用DeleteInstance删除实例。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DeleteInstance&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteInstance|要执行的操作。取值：

 **DeleteInstance**。 |
|InstanceId|String|是|alikafka\_post-cn-mp919o4v\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DeleteInstance
&InstanceId=alikafka_post-cn-mp919o4v****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteInstanceResponse>
      <Message>operation success.</Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015****</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</DeleteInstanceResponse>
```

`JSON` 格式

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015****",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

