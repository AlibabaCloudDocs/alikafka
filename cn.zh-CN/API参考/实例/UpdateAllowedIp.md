# UpdateAllowedIp

调用UpdateAllowedIp变更IP白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=UpdateAllowedIp&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpdateAllowedIp|要执行的操作。取值：

 **UpdateAllowedIp**。 |
|AllowedListIp|String|是|0.0.0.0/0|IP列表。可以是网段，例如：**192.168.0.0/16**。

 -   当**UpdateType**为**add**时，可以填写多项，以英文逗号（,）分隔。
-   当**UpdateType**为**delete**时，一次只能填写单项。
-   删除需谨慎。 |
|AllowedListType|String|是|vpc|白名单的类型。取值：

 -   **vpc**：专有网络VPC。
-   **internet**：公网。 |
|InstanceId|String|是|alikafka\_pre-cn-0pp1cng20\*\*\*|实例ID。 |
|PortRange|String|是|9092/9092|端口范围。取值：

 -   **9092/9092**：专有网络VPC。
-   **9093/9093**：公网。

 该参数需要与**AllowdedListType**对应。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|UpdateType|String|是|add|变更类型。取值：

 -   **add**：增加。
-   **delete**：删除。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|17D425C2-4EA3-4AB8-928D-E10511ECF\*\*\*|请求ID。 |
|Success|Boolean|true|请求是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UpdateAllowedIp
&RegionId=cn-hangzhou
&UpdateType=add
&PortRange=9092/9092
&AllowedListType=vpc
&AllowedListIp=0.0.0.0/0
&InstanceId=alikafka_pre-cn-0pp1cng20***
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<UpdateAllowedIpResponse>
      <RequestId>17D425C2-4EA3-4AB8-928D-E10511ECF***</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <Success>true</Success>
</UpdateAllowedIpResponse>
```

`JSON` 格式

```
{
    "RequestId": "17D425C2-4EA3-4AB8-928D-E10511ECF***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

