# ReleaseInstance

调用ReleaseInstance接口释放后付费实例。

预付费实例不支持使用该接口释放。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=ReleaseInstance&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ReleaseInstance|要执行的操作。取值：

 **ReleaseInstance**。 |
|InstanceId|String|是|alikafka\_post-cn-mp919o4v\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|ForceDeleteInstance|Boolean|否|false|是否立即释放实例的物理资源。取值：

 -   **true** ：立即释放实例的物理资源。
-   **false**：实例物理资源会保留一段时间才释放。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015A\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ReleaseInstance
&InstanceId=alikafka_post-cn-mp919o4v****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<ReleaseInstanceResponse>
      <Message>operation success.</Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015A***</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</ReleaseInstanceResponse>
```

`JSON` 格式

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015A***",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

