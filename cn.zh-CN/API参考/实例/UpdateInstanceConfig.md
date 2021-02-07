# UpdateInstanceConfig

调用UpdateInstanceConfig接口更新实例配置。

## **权限说明**

RAM用户需要先获取授权，才能调用**UpdateInstanceConfig**接口。授权的详细信息，请参见[RAM权限策略](~~185815~~)。

|API

|Action

|Resource |
|-----|--------|----------|
|UpdateInstanceConfig

|alikafka: UpdateInstance

|acs:alikafka:\*:\*:\{instanceId\} |

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=UpdateInstanceConfig&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpdateInstanceConfig|系统规定参数。取值：**UpdateInstanceConfig**。 |
|Config|String|是|\{"kafka.log.retention.hours":"33"\}|需要更新的消息队列Kafka版的配置。配置信息必须是一个合法的JSON字符串。 |
|InstanceId|String|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|实例的ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |

## Config参数说明

|名称

|类型

|取值范围

|默认值

|描述 |
|----|----|------|-----|----|
|enable.vpc\_sasl\_ssl

|Boolean

|true/false

|false

|是否开启VPC传输加密，如果要开启，必须同时开启ACL。 |
|enable.acl

|Boolean

|true/false

|false

|是否开启ACL。 |
|kafka.log.retention.hours

|Integer

|24~480

|72

|消息保留时长（小时）。 |
|kafka.message.max.bytes

|Integer

|1048576~10485760

|1048576

|最大消息大小（字节）。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|4B6D821D-7F67-4CAA-9E13-A5A997C35\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UpdateInstanceConfig
&Config={"kafka.log.retention.hours":"33"}
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<UpdateInstanceConfigResponse>
      <Message>operation success.</Message>
      <RequestId>4B6D821D-7F67-4CAA-9E13-A5A997C35***</RequestId>
      <Code>200</Code>
      <Success>true</Success>
</UpdateInstanceConfigResponse>
```

`JSON`格式

```
{
    "Message": "operation success.",
    "RequestId": "4B6D821D-7F67-4CAA-9E13-A5A997C35***",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

