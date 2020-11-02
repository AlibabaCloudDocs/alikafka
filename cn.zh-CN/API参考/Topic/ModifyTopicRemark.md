# ModifyTopicRemark

调用ModifyTopicRemark修改Topic的备注。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=ModifyTopicRemark&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyTopicRemark|要执行的操作。取值：

 **ModifyTopicRemark**。 |
|InstanceId|String|是|alikafka\_post-cn-0pp1l9z8\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|Topic|String|是|alikafka\_post-cn-0pp1l9z8z\*\*\*|Topic的名称。 |
|Remark|String|否|testremark|备注。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success|返回信息。 |
|RequestId|String|DB6F1BEA-903B-4FD8-8809-46E7E9CE\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ModifyTopicRemark
&InstanceId=alikafka_post-cn-0pp1l9z8***
&RegionId=cn-hangzhou
&Topic=alikafka_post-cn-0pp1l9z8z***
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ModifyTopicRemarkResponse>
      <RequestId>DB6F1BEA-903B-4FD8-8809-46E7E9CE***</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</ModifyTopicRemarkResponse>
```

`JSON` 格式

```
{
    "RequestId":"DB6F1BEA-903B-4FD8-8809-46E7E9CE***",
    "Message":"operation success",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

