# CreatePostPayOrder

调用CreatePostPayOrder创建后付费实例。

请确保在使用该接口前，已充分了解后付费实例的收费方式和价格。更多信息，请参见[计费说明](~~84737~~)。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=CreatePostPayOrder&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreatePostPayOrder|要执行的操作。取值：

 **CreatePostPayOrder**。 |
|DeployType|Integer|是|5|部署类型。取值：

 -   **4**：公网/VPC实例
-   **5**：VPC实例 |
|DiskSize|Integer|是|500|磁盘容量。

 取值范围，请参见[计费说明](~~84737~~)。 |
|DiskType|String|是|0|磁盘类型。取值：

 -   **0**：高效云盘
-   **1**：SSD |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|TopicQuota|Integer|是|50|Topic的数量。

 -   流量规格不同，默认值不同。超过默认值，需额外收费。
-   取值范围，请参见[计费说明](~~84737~~)。 |
|IoMax|Integer|否|20|流量峰值（不推荐）。

 -   流量峰值和流量规格必须选填一个。同时填写时，以流量规格为准。建议您只填写流量规格。
-   取值范围，请参见[计费说明](~~84737~~)。 |
|EipMax|Integer|否|0|公网流量。

 -   如果**DeployType**为**4**，则需填写。
-   取值范围，请参见[计费说明](~~84737~~)。 |
|SpecType|String|否|normal|规格类型。取值：

 -   **normal**：标准版（高写版）
-   **professional**：专业版（高写版）
-   **professionalForHighRead**：专业版（高读版）

 以上规格类型的说明，请参见[计费说明](~~84737~~)。 |
|IoMaxSpec|String|否|alikafka.hw.2xlarge|流量规格（推荐）。

 -   流量峰值和流量规格必须选填一个。同时填写时，以流量规格为准。建议您只填写流量规格。
-   取值范围，请参见[计费说明](~~84737~~)。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|OrderId|String|20497346575\*\*\*\*|订单的ID。 |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|请求的ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreatePostPayOrder
&RegionId=cn-hangzhou
&TopicQuota=50
&DiskType=1
&DiskSize=900
&DeployType=5
&IoMax=20
&SpecType=normal
&IoMaxSpec=alikafka.hw.2xlarge
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<CreatePostPayOrderResponse>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <Message>operation success.</Message>
      <OrderId>20497346575****</OrderId>
      <Code>200</Code>
      <Success>true</Success>
</CreatePostPayOrderResponse>
```

`JSON`格式

```
{
    "RequestId": "06084011-E093-46F3-A51F-4B19A8AD****",
    "Message": "operation success.",
    "OrderId": "20497346575****",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

