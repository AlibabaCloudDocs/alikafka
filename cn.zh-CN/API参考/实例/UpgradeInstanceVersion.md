# UpgradeInstanceVersion

调用UpgradeInstanceVersion接口升配实例版本。

## **权限说明**

RAM用户需要先获取授权，才能调用**UpgradeInstanceVersion**接口。授权的详细信息，请参见[RAM权限策略](~~185815~~)。

|API

|Action

|Resource |
|-----|--------|----------|
|UpgradeInstanceVersion

|UpdateInstance

|acs:alikafka:\*:\*:\{instanceId\} |

## **QPS限制**

单用户请求频率限制为2 QPS。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=UpgradeInstanceVersion&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpgradeInstanceVersion|系统规定参数。取值：**UpgradeInstanceVersion**。 |
|InstanceId|String|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|实例的ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|TargetVersion|String|是|0.10.2|目标开源版本。可取值为：

 -   **0.10.2**
-   **2.2.0**

 如果填写值为同版本，会触发升级到最新小版本。 |

## **TargetVersion参数示例说明**

|升级前开源版本

|升级前小版本

|目标开源版本

|升级后开源版本

|升级后小版本 |
|---------|--------|--------|---------|--------|
|0.10.2

|非最新小版本

|0.10.2

|0.10.2

|最新小版本 |
|0.10.2

|非最新小版本

|2.2.0

|2.2.0

|最新小版本 |
|0.10.2

|最新小版本

|2.2.0

|2.2.0

|最新小版本 |
|2.2.0

|非最新小版本

|2.2.0

|2.2.0

|最新小版本 |

升级小版本详细信息，请参见[升级实例版本](~~113173~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UpgradeInstanceVersion
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&TargetVersion=0.10.2
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<UpgradeInstanceVersionResponse>
      <Message>operation success.</Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015***</RequestId>
      <Code>200</Code>
      <Success>true</Success>
</UpgradeInstanceVersionResponse>
```

`JSON`格式

```
{
    "Message": "operation success.",
    "RequestId": "ABA4A7FD-E10F-45C7-9774-A5236015***",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

