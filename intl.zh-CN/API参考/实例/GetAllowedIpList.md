# GetAllowedIpList

调用GetAllowedIpList获取IP白名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetAllowedIpList&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetAllowedIpList|要执行的操作。取值：

 **GetAllowedIpList**。 |
|InstanceId|String|是|alikafka\_post-cn-mp91inkw\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|AllowedList|Struct| |白名单。 |
|DeployType|Integer|4|部署类型。取值：

 -   **4**：公网/VPC
-   **5**：VPC

 **说明：** 普通用户无需关心该字段，集成商客户可以了解一下。 |
|InternetList|Array of IPListVO| |公网白名单。 |
|AllowedIpList|List|0.0.0.0/0|IP白名单。 |
|PortRange|String|9093/9093|端口范围。取值：

 **9093/9093**。 |
|VpcList|Array of IPListVO| |VPC白名单。 |
|AllowedIpList|List|192.XXX.X.X/XX|IP白名单。 |
|PortRange|String|9092/9092|端口范围。取值：

 **9092/9092**。 |
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|A421CCD7-5BC5-4B32-8DD8-64668A8FCB56|请求的ID。 |
|Success|Boolean|true|请求是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetAllowedIpList
&RegionId=cn-hangzhou
&InstanceId=alikafka_post-cn-mp91inkw****
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<GetAllowedIpListResponse>
        <RequestId>A421CCD7-5BC5-4B32-8DD8-64668A******</RequestId>
        <Message>operation success.</Message>
        <AllowedList>
              <DeployType>4</DeployType>
              <InternetList>
                    <PortRange>9093/9093</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
              </InternetList>
              <VpcList>
                    <PortRange>9094/9094</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
              </VpcList>
              <VpcList>
                    <PortRange>9092/9092</PortRange>
                    <AllowedIpList>0.0.0.0/0</AllowedIpList>
                    <AllowedIpList>192.XXX.X.X/XX</AllowedIpList>
              </VpcList>
        </AllowedList>
        <Code>200</Code>
        <Success>true</Success>
</GetAllowedIpListResponse>
```

`JSON`格式

```
{
	"RequestId": "A421CCD7-5BC5-4B32-8DD8-64668A******",
	"Message": "operation success.",
	"AllowedList": {
		"DeployType": 4,
		"InternetList": [
			{
				"PortRange": "9093/9093",
				"AllowedIpList": [
					"0.0.0.0/0"
				]
			}
		],
		"VpcList": [
			{
				"PortRange": "9094/9094",
				"AllowedIpList": [
					"0.0.0.0/0"
				]
			},
			{
				"PortRange": "9092/9092",
				"AllowedIpList": [
					"0.0.0.0/0",
					"192.XXX.X.X/XX"
				]
			}
		]
	},
	"Code": 200,
	"Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

