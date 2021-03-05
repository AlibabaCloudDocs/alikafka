# DeleteConsumerGroup

调用DeleteConsumerGroup删除Consumer Group。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DeleteConsumerGroup&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteConsumerGroup|要执行的操作。取值：

 **DeleteConsumerGroup**。 |
|ConsumerId|String|是|CID-test|Consumer Group名称。取值：

 -   只能包含字母、数字、短划线（-）、下划线（\_）。
-   长度限制在3字符~64字符，多于64字符将被自动截取。
-   Consumer Group名称创建后，将不能修改。 |
|InstanceId|String|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|请求的ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DeleteConsumerGroup
&ConsumerId=CID-test
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<DeleteConsumerGroupResponse>
      <Message>operation success.</Message>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</DeleteConsumerGroupResponse>
```

`JSON`格式

```
{
    "RequestId":"06084011-E093-46F3-A51F-4B19A8AD****",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

