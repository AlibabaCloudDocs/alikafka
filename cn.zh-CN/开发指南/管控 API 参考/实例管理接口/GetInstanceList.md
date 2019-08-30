# GetInstanceList {#doc_api_alikafka_GetInstanceList .reference}

调用 GetInstanceList 获取您在某地域下所购买的实例的信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetInstanceList|要执行的动作。取值：**GetInstanceList**

 |
|RegionId|String|是|cn-hangzhou|实例所属的地域 ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码，返回 “200” 代表成功。

 |
|InstanceList| | |实例列表信息。

 |
|CreateTime|Long|1566215995000|创建时间。

 |
|EndPoint|String|192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:\*\*\*\*|默认接入点。

 |
|ExpiredTime|Long|1568908800000|过期时间。

 |
|InstanceId|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例 ID。

 |
|Name|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例名称。

 |
|RegionId|String|cn-hangzhou|实例所属的地域 ID。

 |
|ServiceStatus|Integer|5|服务状态。

 |
|VSwitchId|String|vsw-bp13rg6bcpkxofr78\*\*\*\*|VSwitch ID。

 |
|VpcId|String|vpc-bp1l6hrlykj3405r7\*\*\*\*|VPC ID。

 |
|Message|String|operation success.|返回信息。

 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*\*|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetInstanceList
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetInstanceListResponse>
      <Message>operation success.</Message>
      <RequestId>0220FACD-4D57-4F46-BA77-AD333498****</RequestId>
      <Success>true</Success>
      <Code>200</Code>
      <InstanceList>
            <InstanceVO>
                  <Name>alikafka_pre-cn-mp919o4v****</Name>
                  <DeployType>4</DeployType>
                  <CreateTime>1566215995000</CreateTime>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-mp919o4v****</InstanceId>
                  <SslEndPoint>47.111.110.11:9093,121.40.96.141:9093,47.111.118.133:****</SslEndPoint>
                  <EndPoint>192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:****</EndPoint>
                  <ExpiredTime>1568908800000</ExpiredTime>
                  <VSwitchId>vsw-bp13rg6bcpkxofr78****</VSwitchId>
                  <VpcId>vpc-bp1l6hrlykj3405r7****</VpcId>
                  <UpgradeServiceDetailInfo>
                        <Current2OpenSourceVersion>0.10</Current2OpenSourceVersion>
                  </UpgradeServiceDetailInfo>
                  <ServiceStatus>5</ServiceStatus>
            </InstanceVO>
      </InstanceList>
</GetInstanceListResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success.",
	"RequestId":"0220FACD-4D57-4F46-BA77-AD333498****",
	"Success":true,
	"Code":200,
	"InstanceList":{
		"InstanceVO":[
			{
				"Name":"alikafka_pre-cn-mp919o4v****",
				"DeployType":4,
				"InstanceId":"alikafka_pre-cn-mp919o4v****",
				"RegionId":"cn-hangzhou",
				"CreateTime":1566215995000,
				"SslEndPoint":"47.111.110.11:9093,121.40.96.141:9093,47.111.118.133:****",
				"EndPoint":"192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:****",
				"VSwitchId":"vsw-bp13rg6bcpkxofr78****",
				"ExpiredTime":1568908800000,
				"VpcId":"vpc-bp1l6hrlykj3405r7****",
				"ServiceStatus":5,
				"UpgradeServiceDetailInfo":{
					"Current2OpenSourceVersion":"0.10"
				}
			}
		]
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

